pipeline {
  agent none

  options {
    gitlabBuilds(builds: ['Linting', 'Build-env', 'Build ood', 'Build compute', 'Build ohpc'])
  }

  stages {
    stage('Linting') {
      agent {
        docker {
          image 'cytopia/pylint'
          args '--entrypoint= '
        }
      }
      steps {
        script {
          echo "RESULT: ${currentBuild.result}"
          try {
            sh 'pylint *.py'
          } catch (err) {
            echo err.getMessage()
            unstable('Linting failed')
            echo "RESULT: ${currentBuild.result}"
          }
        }
      }
      post {
        success {
          updateGitlabCommitStatus name: 'Linting', state: 'success'
        }
        unstable {
          // workaround for allow failure
          // updateGitlabCommitStatus only support pending, running, canceled, success, failed
          // So, use success state with failed name to indicate linting failed but continue CI pipeline
          updateGitlabCommitStatus name: 'Linting', state: 'failed'
        }
      }
    }
  }
}