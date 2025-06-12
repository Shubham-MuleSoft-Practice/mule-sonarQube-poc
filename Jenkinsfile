pipeline
{
	agent any
	stages{
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
			    echo '***** check sonarQube analysis *****'
                bat 'mvn clean verify sonar:sonar -Dsonar.projectKey=mule-test-01 -Dsonar.projectName='mule-test-01' -Dsonar.host.url=http://192.168.1.147:9000 -Dsonar.token=sqp_9e7f6a4105876055fbafe351575b3aedb5d08e86 -Dsonar.java.binaries=target\\classes'
            }
        }
		stage('Deploy CloudHub 2.0') {
			steps {
			   bat 'mvn clean deploy -DmuleDeploy
			}
		}
    }
	 
}

