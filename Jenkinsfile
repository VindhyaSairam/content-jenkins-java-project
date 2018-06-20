pipeline {

  agent none

  stages {


    stage('Unit Tests') {
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

    stage('deploy') {
      agent {
        label 'master'
      }

      steps {
        sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangle/all/"
      }

    }

    stage('deploy on slave-FT') {
      agent {
        label 'slave1'
      }

      steps {
        sh "wget http://ec2-34-205-127-163.compute-1.amazonaws.com/rectangle/all/rectangle_${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 5 6"
      }
    }

    stage("Test on Ubuntu") {
      agent {
	label 'slave1'
        docker 'ubuntu:16.04'
      }
      steps {
        sh "wget http://ec2-34-205-127-163.compute-1.amazonaws.com/rectangle/all/rectangle_${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 2 4"
      }
    }

  }

}
