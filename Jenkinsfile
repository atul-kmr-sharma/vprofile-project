pipeline {
    agent any
    tools {
	    maven "MAVEN3"
	    jdk "OracleJDK8"
	}

    environment {
        registryCredential = 'ecr:us-east-1:awscreds'
        appRegistry = "304182266883.dkr.ecr.us-east-1.amazonaws.com/vprofile"
        vprofileRegistry = "https://304182266883.dkr.ecr.us-east-1.amazonaws.com"
    }
  stages {
    stage('Fetch code'){
      steps {
        git branch: 'docker', url: 'https://github.com/atul-kmr-sharma/vprofile-project.git'
      }
    }


    stage('Test'){
      steps {
        sh 'mvn test'
      }
    }

    stage ('CODE ANALYSIS WITH CHECKSTYLE'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
            post {
                success {
                    echo 'Generated Analysis Result'
                }
            }
        }


    stage('Build App Image') {
       steps {
         script {
                dockerImage = docker.build(appRegistry + ":$BUILD_NUMBER", "--rm ./Docker-files/app/multistage/")
             }
     }
        }

    stage('Upload App Image') {
          steps{
            script {
                docker.withRegistry( vprofileRegistry, registryCredential ) {
                dockerImage.push("$BUILD_NUMBER")
                dockerImage.push('latest')
              }
            }
            
          }

    post {
                success {
                    echo 'Image Uploaded Successfully'
                    sh 'docker system prune -af'
                }
            }
     }

  }
}