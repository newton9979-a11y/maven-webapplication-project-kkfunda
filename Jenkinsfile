pipeline {
    agent any

    tools {
        maven 'maven-3.9.9'
    }

    triggers {
        githubPush()
    }

    stages {

        stage('Code Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/newton9979-a11y/spring-boot-mongo-docker-kkfunda.git'
            }
        }

        stage('Verify Tools') {
            steps {
                sh '''
                    java -version
                    mvn -version
                '''
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Unit Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    withCredentials([
                        string(
                            credentialsId: 'sonartoken',
                            variable: 'SONAR_TOKEN'
                        )
                    ]) {
                        sh '''
                            mvn sonar:sonar \
                              -Dsonar.projectKey=spring-boot-mongo \
                              -Dsonar.projectName=spring-boot-mongo \
                              -Dsonar.token=$SONAR_TOKEN
                        '''
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check Jenkins console output.'
        }
    }
}
