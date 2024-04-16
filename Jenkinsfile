pipeline {
  agent {
    label 'agent'
  }

  tools {
    maven "Maven-3.9.6"
  }

  environment {
                ScannerHome = tool 'Sonar-5'
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

    stage('Code Analysis') {
      steps {
        script {
          withSonarQubeEnv(credentialsId: 'SONAR-TK') {
            sh '''
            echo start of code scan
            ${ScannerHome}/bin/sonar-scanner -Dsonar.projectKey=Jomacs_Webapp
            echo end of code scan
            '''
          }
        }
      }
    }

    stage("Quality Gate") {
        steps {
            timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
            }
        }
    }

    stage('Deploy to Nexus') {
      steps {
        nexusArtifactUploader artifacts: [[artifactId: 'maven-web-application', classifier: '', file: 'target/web-app.war', type: 'war']], credentialsId: 'NEXUS_CRED', groupId: 'com.mt', nexusUrl: '54.197.131.35:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'WebApp-Release', version: '3.1.2-RELEASE'
      }
    }

    stage('Deply to Tomcat') {
      steps {
        deploy adapters: [tomcat9(credentialsId: 'TOMCAT', path: '', url: 'http://54.157.246.139:8080/')], contextPath: null, war: 'target/*.war'
      }
    }

  }
}