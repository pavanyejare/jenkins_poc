pipeline {
    agent {
        label 'app-server' 
    }
    stages {
	stage('Code'){
		steps{
			sh 'rm -rf $WORKSPACE/*'
		        checkout ([$class: 'GitSCM', branches: [[name: '*/master']], userRemoteConfigs: [[url: 'https://github.com/pavanyejare/jenkins_poc.git']], ])
		}
	}
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deploy') {
            steps {
                sh '''
  		      mvn jar:jar install:install help:evaluate -Dexpression=project.name
		      NAME=`mvn help:evaluate -Dexpression=project.name | grep "^[^\\[]"`
		      VERSION=`mvn help:evaluate -Dexpression=project.version | grep "^[^\\"`
                      java -jar target/${NAME}-${VERSION}.jar
		  '''
            }
        }
    }
}
