#!groovy

def channel = '#devops-builds'

properties(
  [[$class: 'GithubProjectProperty', projectUrlStr: 'https://github.com/hmcts/vnet-peers-role/'],
   pipelineTriggers([[$class: 'GitHubPushTrigger']])]
)

@Library('Reform') _

node {
  ws('vnet-peers-role') {
    try {
      wrap([$class: 'AnsiColorBuildWrapper', colorMapName: 'xterm']) {
        stage('Checkout') {
          checkout scm
        }
        // Placeholder for potential az-wrapper check.
        // TODO
        //stage('Create docker container.') {
        //  sh '''
        //    docker run --rm --name "vnetpeersrole" devops/centos echo "hello world"
        //  '''
        //}

        stage('Vars Checkout') {
          checkout scm
          dir('ansible-management') {
          git url: "git@git.reform.hmcts.net:devops/ansible-management.git", branch: "master"
          }
        }

        stage('Lint vars') {
          sh '''
            ls -l
            yamllint ansible-management/vnet-peers-vars/main.yml
            ls -l ansible-management/
          '''
        }

      }

    } catch (err) {
      notifyBuildFailure channel: "${channel}"
      throw err
    } finally {
      stage('Cleanup') {
          sh '''
            echo "Nothing to do for Cleanup."
            '''
        }
        deleteDir()
    }
  }
}
