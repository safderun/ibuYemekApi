pipeline{
  agent any
  environment {
    dockerhub=credentials('dockerhub')
  }

  stages {
    stage('docker build') {
      steps {
        sh 'docker build -t safderun/ibu-yemek-api:build .'
      }
    }

    stage('master-branch-stuff') {
      when {
          branch 'master'
      }
      steps {
        echo 'run this stage - ony if the branch = master branch'
      }
    }

    stage ('development docker push') {
      steps{
        sh 'echo $dockerhub_PSW | docker login -u $dockerhub_USR --password-stdin'
        sh 'docker tag safderun/ibu-yemek-api:build safderun/ibu-yemek-api:dev'
        sh 'docker push safderun/ibu-yemek-api:dev'
        mail bcc: '', body: 'IBU Yemek API project docker pushed succesfully!', cc: '', from: 'Jenkins', replyTo: '', subject: 'ibuYemekApi Build', to: 'safderun@proton.me'
      }
    }

    stage('deploy') {
      // def dockerPull = ''
      // def dockerRun = ''
      sshagent(credentials: ['ec2-jenkins-agent']) {
        steps {
          sh 'ssh -o StrictHostKeyChecking=no admin@ec2-3-72-108-27.eu-central-1.compute.amazonaws.com docker pull safderun/ibu-yemek-api:dev'
          sh 'ssh -o StrictHostKeyChecking=no admin@ec2-3-72-108-27.eu-central-1.compute.amazonaws.com docker container run -p 3000:3000 --name ibu-yemek-api-dev -d safderun/ibu-yemek-api:dev' 
        }
      }
    }
  }
}

