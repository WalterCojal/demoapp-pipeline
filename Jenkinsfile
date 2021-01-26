pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Build stage'
        sh 'sh run_build_script.sh'
      }
    }

    stage('Linux tests') {
      parallel {
        stage('Linux tests') {
          steps {
            echo 'Run linux tests'
            sh 'sh run_linux_tests.sh'
          }
        }

        stage('Windows tests') {
          steps {
            echo 'Run Windows tests'
          }
        }

      }
    }

    stage('Deploy staging') {
      steps {
        echo 'Deploy staging'
        input 'Ok to deploy'
      }
    }

    stage('Deploy prod') {
      steps {
        echo 'Deploy to prod'
      }
    }

  }
  post {
    always {
      archiveArtifacts(artifacts: 'target/demoapp.jar', fingerprint: true)
    }

    failure {
      mail(to: 'ci-team@example.com', subject: "Failed Pipeline ${currentBuild.fullDisplayName}", body: " For details about the failure, see ${env.BUILD_URL}")
    }

  }
}