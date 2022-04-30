pipeline{
  agent any
  environment {
    dockerhub=credentials('dockerhub')
  }

  stages {
    stage('scm checkout') {

      steps {
        git(credentialsId: 'jenkins-github', url: 'git@github.com:safderun/ibuYemekApi.git', branch: 'master')
      }

    }

    stage('docker build') {
      steps {
        sh 'docker build -t safderun/ibu-yemek-api:build .'
      }
    }

    stage ('master docker push') {
      steps{
        script{
          echo 'checking branch'
          echo ${GIT_BRANCH}
          if ('${GIT_BRANCH}' == 'origin/master') {
            echo 'pushing to dockerhub'
            sh 'echo $dockerhub_PSW | docker login -u $dockerhub_USR --password-stdin'
            sh 'docker tag safderun/ibu-yemek-api:build safderun/ibu-yemek-api:latest'
            sh 'docker push safderun/ibu-yemek-api:latest'
          }
        }
        mail bcc: '', body: 'IBU Yemek API project docker pushed succesfully!', cc: '', from: 'Jenkins', replyTo: '', subject: 'ibuYemekApi Build', to: 'safderun@proton.me'
      }
    }

    // stage ('master docker push') {
    //   when {
    //     branch 'origin/master'
    //   }
    //   steps{
    //     sh 'echo $dockerhub_PSW | docker login -u $dockerhub_USR --password-stdin'
    //     sh 'docker tag safderun/ibu-yemek-api:build safderun/ibu-yemek-api:latest'
    //     sh 'docker push safderun/ibu-yemek-api:latest'
    //     mail bcc: '', body: 'IBU Yemek API project docker pushed succesfully!', cc: '', from: 'Jenkins', replyTo: '', subject: 'ibuYemekApi Build', to: 'safderun@proton.me'
    //   }
    // }

    // stage ('dev docker push') {
    //   when {
    //     branch 'origin/dev'
    //   }
    //   steps{
    //     sh 'echo $dockerhub_PSW | docker login -u $dockerhub_USR --password-stdin'
    //     sh 'docker tag safderun/ibu-yemek-api:build safderun/ibu-yemek-api:dev'
    //     sh 'docker push safderun/ibu-yemek-api:dev'
    //     mail bcc: '', body: 'IBU Yemek API project docker pushed succesfully!', cc: '', from: 'Jenkins', replyTo: '', subject: 'ibuYemekApi Build', to: 'safderun@proton.me'
    //   }
    // }

    stage('deploy') {
      steps {
        echo 'Deploying...'
      }
    }
  }
}

