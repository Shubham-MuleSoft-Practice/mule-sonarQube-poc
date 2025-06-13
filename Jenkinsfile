pipeline {
    agent any
	environment {
        MAVEN_SETTINGS = credentials('maven-settings')
    }

    stages {
        stage('Build Application') {
            steps {
                echo '***** Building application *****'
                bat 'mvn clean deploy'
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
                    -test-01 ^
                    -Dsonar.projectName=mule-test-01 ^
                    -Dsonar.host.url=http://192.168.1.147:9000 ^
                    -Dsonar.token=sqp_9e7f6a4105876055fbafe351575b3aedb5d08e86 ^mvn clean verify sonar:sonar ^
                    -Dsonar.projectKey=mule
                    -Dsonar.java.binaries=target\\classes
                """
            }
        }

        stage('Deploy CloudHub 2.0') {
            steps {
               echo '***** Deploy to cloudHub2.0 *****'
			   configFileProvider([configFile(fileId: 'maven-settings', variable: 'MAVEN_SETTINGS_FILE')]) {
                    bat """
                        mvn clean deploy -DmuleDeploy -s %MAVEN_SETTINGS_FILE%
                    """
                }
            }
        }
    }
}