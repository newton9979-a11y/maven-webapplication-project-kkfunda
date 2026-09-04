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
                    echo "Java:"
                    java -version

                    echo "Maven:"
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
                            mvn org.sonarsource.scanner.maven:sonar-maven-plugin:sonar \
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
            echo '====================================='
            echo 'Pipeline completed successfully!'
            echo '====================================='
        }

        failure {
            echo '====================================='
            echo 'Pipeline failed!'
            echo 'Check Jenkins console output.'
            echo '====================================='
        }
    }
}
