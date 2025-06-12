pipeline {
    agent any

    stages {
        stage('Build Application') {
            steps {
                echo '***** Building application *****'
                bat 'mvn clean deploy -U -Dlog4j2.loggerContextFactory=org.apache.logging.log4j.simple.SimpleLoggerContextFactory -Dmunit.failIfNoTests=false -s C:\\Users\\Admin\\.m2\\settings.xml'
            }
        }

        stage('Run MUnit Tests') {
            steps {
                echo '***** Running MUnit test cases *****'
                bat 'mvn clean test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo '***** Check SonarQube analysis *****'
                bat """
                    mvn clean verify sonar:sonar ^
                    -Dsonar.projectKey=Shubham-MuleSoft-Practice_mule-sonarQube-poc ^
                    -Dsonar.projectName=mule-sonarQube-poc ^
                    -Dsonar.host.url=https://sonarcloud.io ^
                    -Dsonar.token=428647a94ab058d3aa1d495d5c82f511c5a52a48 ^
                    -Dsonar.java.binaries=target\\classes
                """
            }
        }

        stage('Deploy CloudHub 2.0') {
            steps {
                bat 'mvn clean deploy -DmuleDeploy'
            }
        }
    }
}
