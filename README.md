# declarative-multi-branch-with-parallel
Declarative Multibranch Pipeline With Seq & Parallel Sub Stages


```
pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage("checkout") {
            steps {
                snDevOpsStep()
                snDevOpsChange()
                echo "Building" 
                checkout scm
            }
        }

        stage("build") {
            steps {
                snDevOpsStep()
                snDevOpsChange()
                echo "Building" 
                sh "mvn clean install -DskipTests=true"
            }
        }

        stage("test") {
            parallel {
                stage('Unit Test') {
                    steps {
                        snDevOpsStep()
                        snDevOpsChange()
                        echo "Unit Test"
                        sh "mvn test -Dtest=AppTest"
                    }
                }
                stage('UAT Test') {
                    steps {
                        snDevOpsStep ()
                        snDevOpsChange()
                        echo "Testing"
                        sh "mvn test -Dtest=AddTest"
                    }
                }
            }
        }

        stage("integration-test") {
            steps {
                snDevOpsStep ()
                snDevOpsChange()
                echo "Testing"
                sh "mvn test -Dtest=SubtractTest"
            }
        }

        stage("deploy") {
            stages{
                stage('deploy to dev') {
                    when{
                        branch 'dev'
                    }
                    steps{
                        snDevOpsStep ()
                        echo "deploy in UAT"
                        snDevOpsChange()
                    }
                }
                stage('deploy to prod') {
                    when {
                        branch 'master'
                    }
                    steps{
                        snDevOpsStep ()
                        echo "deploy in prod"
                        snDevOpsChange()
                    }
                }
            }
        }
    }

}
```
