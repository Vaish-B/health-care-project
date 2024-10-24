pipeline {
  agent any
     tools {
       maven 'M2_HOME'
           }

  stages {
    stage('Git Checkout') {
      steps {
        echo 'This stage is to clone the repo from github'
             git branch: 'master', url: 'https://github.com/Vaish-B/health-care-project.git'
            }
            }
   stage('Create Package') {
      steps {
        echo 'This stage will compile, test, package my application'
                sh 'mvn package'

            }        
        }
       stage('Generate Test Reports') {
           steps {
               publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/var/lib/jenkins/workspace/Healthcare/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                    }
            }
    stage('Create Docker Image') {
      steps {
        echo 'This stage will Create a Docker image'
        sh 'docker build -t vaishu1998/healthcare:1.0 .'
                }
            }
     stage('Login to Dockerhub') {
      steps {
        echo 'This stage will loginto Dockerhub' 
       withCredentials([usernamePassword(credentialsId: 'Docker-login', passwordVariable: 'docker-pass', usernameVariable: 'docker-login')]) {
        sh 'docker login -u ${Docker-login} -p ${docker-pass}'
            }
         }
     }
     stage('Docker Push-Image') {
      steps {
        echo 'This stage will push my new image to the dockerhub'
        sh 'docker push vaishu1998/healthcare:1.0'
            }
      }
    }
}
