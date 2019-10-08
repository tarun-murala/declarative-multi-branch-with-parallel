pipeline {
    agent any
    tools { 
        maven 'Maven' 
    }
    stages {
        stage('Checkout') {
            steps {
                snDevOpsStep()
                snDevOpsChange()
                checkout scm
            }
        }
        stage('Build') {
            steps {
                snDevOpsStep()
                snDevOpsChange()
                sh 'mvn clean install -DskipTests=true'
            }
        }
        stage('Unit Test') {
            steps {
                snDevOpsStep()
                snDevOpsChange()
                sh "mvn test -Dtest=AppTest"
            }
        }
        stage('Integration Test') {
            steps {
                snDevOpsStep()
                snDevOpsChange()
                sh "mvn test -Dtest=AddTest"
            }
        }
        stage('Deploy to Dev') {
            when {
                branch 'dev' 
            }
            steps {
                snDevOpsStep()
                snDevOpsChange()
                // sh "mvn -B deploy"
                // sh "mvn -B release:prepare"
                // sh "mvn -B release:perform"
                // deploy using kubernetes - kubectl
                echo "Deploy to dev"
            }
        }
        stage('Deploy to Prod') {
            when {
                branch 'master'  
            }
            steps {
                snDevOpsStep()
                snDevOpsChange()
                // sh "mvn -B deploy"
                // sh "mvn -B release:prepare"
                // sh "mvn -B release:perform"
                // deploy using kubernetes - kubectl
                echo "Deploy to prod"
            }
        }
    }
}