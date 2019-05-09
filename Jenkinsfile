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
        sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangle"
      }

    }

    stage('FT-test on slave') {
      agent {
        label 'tomcat'
      }

      steps {
        sh "wget http://ec2-18-212-36-246.compute-1.amazonaws.com/rectangle/rectangle_${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 5 6"
      }
    }


  }
}
