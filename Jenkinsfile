pipeline {
 agent {
    kubernetes {
      label 'python-app'
      yamlFile 'build-pod.yaml'
    }
  }
 
  stages {
    stage('Build') {
      steps {
        container ('python') {
          sh 'pip install -r requirements.txt'
        }

      }
    }
    stage ('Deploy to Testing Environment') {
      steps {
        container ('python') {
          echo "deploy target/*.war to testing environment"
        }
      }
    }

           
    stage('Scan') {
      steps {
        container ('java') {
          synopsys_detect(detectProperties: '--blackduck.url="https://bizdevhub.blackducksoftware.com"  --blackduck.api.token="${BLACKDUCK_ACCESS_TOKEN}"', returnStatus: true)
        }
      }
    }

  }

  environment {
    BLACKDUCK_ACCESS_TOKEN = credentials('jenkins-blackduck-access-token')
  }
 } 
