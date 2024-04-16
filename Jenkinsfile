pipeline {
  agent {
    label 'agent'
  }

  tools {
    maven "Maven-3.9.6"
  }

  stages {
    stage('Git Clone') {
      steps {
        sh "echo start of git clone"
        git branch: 'main', url: 'https://github.com/gyenoch/web-app.git'
        sh "echo end of git clone"
      }
    }

    stage('Maven Build') {
      steps {
        sh "echo start building from Maven"
        sh "mvn clean package"
        sh"echo end of build"
      }
    }
  }
}