# Jenkins



参考:

- [jenkins文档](https://www.jenkins.io/zh/doc/)


<br/>

<!--more-->

<br/>



# 入门

Jenkins是一款开源的CICD软件，用于自动化各种任务。

Jenkins有很多插件，可以帮助完成许多工作。


<br/>


## 入门指南

准备工作:

- 机器要求
  - 内存512MB+
  - 磁盘10GB+
- 软件要求
  - Java8
  - Docker

<br/>

下载并运行jenkins:

```
mkdir /opt/jenkins
cd /opt/jenkins
wget http://mirrors.jenkins.io/war-stable/latest/jenkins.war

java -jar jenkins.war --httpPort=8080
```


<br/>
<br/>


## 创建第一个流水线

Jenkins Pipeline是一套插件，将持续交付的实现和实施集成到Jenkins中。Jenkins Pipeline通常写入`Jenkinsfile`的文本文件中，放入版本控制的仓库中。

可直接在Jenkins界面上创建作业，编写流水线，保存并运行。

```Jenkinsfile
pipeline {
	agent any

	stages {
    	stage('Build') {
        	steps {
            	echo 'Building...
            }
        }
        stage('Test') {
        	steps {
            	echo 'Testing...
            }
        }
        stage('Deploy') {
        	steps {
            	echo 'Deploying...'
            }
        }
    }
}
```


<br/>
<br/>


## 执行多个步骤

流水线由多个步骤组成。可以把步骤看作一个执行单一动作的单一的命令。当一个步骤运行成功时继续执行下一个步骤。当任何一个步骤失败时，流水线的执行结果也视为失败。

如基于Unix-Like系统中的shell命令，对应的`sh`步骤。

```Jenkinsfile
pipeline {
	agent any
    stages {
    	stage('Build') {
        	steps {
            	sh 'echo "Hello World"'
                sh '''
                	echo "Multiline shell steps"
                    ls -lah
                '''
            }
        }
    }
}
```

<br/>

超时和重试等。

```Jenkinsfile
pipeline {
	agent any
    stages {
    	stage('Deploy') {
        	steps {
            	retry(3) {
                	sh './xxx.sh'
                }
                timeout(time: 3, unit: 'MINUTES') {
                	sh './health-check.sh'
                }
            }
        }
    }
}
```

<br/>

完成时的一些动作。

```Jenkinsfile
pipeline {
	agent any
    stages {
    	stage('Test') {
        	steps {
            	sh 'echo "Fail!"; exit 1'
            }
        }
    }
    post {
    	always {
        	echo '总是会运行'
        }
        success {
        	echo '成功才运行'
        }
        failure {
        	echo '失败才运行'
        }
        unstable {
        	echo '被标记为unstable才运行'
        }
        changed {
        	echo '流水线状态改变才运行'
            echo '例如，流水线之前失败，现在成功'
        }
    }
}
```


<br/>
<br/>


## 定义执行环境

`agent`指令告诉Jenkins在哪里以及如何执行流水线，所有的流水线都需要`agent`指令。

如使用Docker运行：

```Jenkinsfile
pipeline {
	agent {
    	docker {image 'node:7-alpine'}
    }
    stages {
    	stage('Test') {
        	steps {
            	sh 'node --version'
            }
        }
    }
}
```


<br/>
<br/>


## 使用环境变量

环境变量既可以是全局的，也可以是阶段级别的。

```Jenkinsfile
pipeline {
	agent any

    environment {
    	DISABLE_AUTH = 'true'
        DB_ENGINE = 'sqlite'
    }
}
```


<br/>
<br/>


## 记录测试和构建结果

虽然测试是良好的持续交付过程中的关键部分，但大多数人并不希望筛选数千行控制台输出来查找有关失败测试的信息。 为了简化操作，只要您的测试运行时可以输出测试结果文件，Jenkins 就可以记录和汇总这些测试结果。

Jenkins通常与`junit`步骤捆绑在一起，但如果您的测试运行结果无法输出 JUnit 样式的 XML 报告， 那么还有其他插件可以处理任何广泛使用的测试报告格式。

```Jenkinsfile
pipeline {
	agent any
    stages {
    	stage('Test') {
        	steps {
            	sh './gradlew check'
            }
        }
    }
    post {
    	always {
        	junit 'build/reports/**/*.xml'
        }
    }
}
```


<br/>
<br/>


## 清理和通知

在`post`部分添加通知、清理或其它步骤。


<br/>
<br/>


## 部署

一个常见的模式是扩展阶段的数量以获取额外的部署环境信息。

人工确认，是否可以继续运行。使用`input`步骤来完成。

```Jenkinsfile
pipeline {
	agent any
    stages {
    	/* "Build" 和 "Test" 这里忽略了 */
        stage('Deploy - Staging') {
        	steps {
            	sh './deploy staging'
                sh './run-smoke-tests'
            }
        }
        stage(Sanity check) {
        	steps {
            	input "Does the staging environment look ok?"
            }
        }
        state('Deploy - Production') {
        	steps {
            	sh './deploy production'
            }
        }
    }
}
```




<br/>

---

<br/>




# 用户手册






































