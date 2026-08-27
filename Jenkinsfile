Piprline{

    tools{
       maven 'Maven-3.9.9'
    }

    stages{
        stage('code checkout')
            steps{
                echo 'checking tthe out source code...'

                git(
                    branch: 'master',
                    newton9979-a11y: 'newton9979-a11y',
                    url: 'https://github.com/newton9979-a11y/maven-webapplication-project-kkfunda.git'
                )
            }
    }
        
}