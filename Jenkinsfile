pipeline {

  agent none

  stages {


    stage('Unit Test') {
      agent {
        label 'master'
      }

      steps {
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }
    }

    stage('build') {
      agent {
      label 'master'
      }

      steps {
        sh 'ant -f build.xml -v'
      }
      post {
        success {
          archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
        }
      }

    }  
    
    stage('deploy-apache') {
      agent {
        label 'master'
      }

      steps {
        sh "scp dist/rectangle_${env.BUILD_NUMBER}.jar jenkins@ec2-3-86-115-138.compute-1.amazonaws.com:/var/www/html/rectangle"
      }

    }

  }
}
