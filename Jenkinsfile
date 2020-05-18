def projectName = 'berlin-clock'
def projectKey = 'berlin-clock'
pipeline {
    agent any
    tools {
        maven 'maven3'
        jdk 'jdk11'
    }
    stages {
        
		    stage('SCM') {
            steps{
           git 'https://github.com/ssqasim-cc/K8s-CI-Jenkins.git'
            }
        }

		stage ('Initialize') {           
		   steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }		
        stage ('Build') {
            steps {
                parallel(install: {
                sh 'mvn -Dmaven.test.failure.ignore=true install' 
                },  sonar: {
                    sh "mvn sonar:sonar -Dsonar.host.url=${env.SONARQUBE_HOST}"
                })

            }
            post {
                success {
                    junit 'target/surefire-reports/**/*.xml' 
                }
            }
        }

    }
}
