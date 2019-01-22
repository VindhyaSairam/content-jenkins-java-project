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
        sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangle/"
      }

    }
    
    stage('deploy on slave-FT') {
      agent {
        label 'slave'
      }

      steps {
        sh "wget http://ec2-3-88-255-14.compute-1.amazonaws.com/rectangle/rectangle_${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 5 6"
      }
    
    }

    stage("Test on UbuntuDocker-FT") {
      agent {
        docker {
          image 'ubuntu:latest'
          label 'docker'
        }
      }
      steps {
        sh "wget http://ec2-3-88-255-14.compute-1.amazonaws.com/rectangle/rectangle_${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 2 4"
      }
    }

  }
}
