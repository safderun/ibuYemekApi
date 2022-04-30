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
      steps {
        echo 'Deploying...'
      }
    }
  }
}

