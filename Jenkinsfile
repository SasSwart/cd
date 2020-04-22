pipeline {
  agent any
  environment {
      DEPLOY_KEY = credentials('cd-deploy-key')
      GIT_SSH_COMMAND = 'ssh -i ./key'
  }
  stages {
    stage('Pull Repository') {
      steps {
        sh "echo $DEPLOY_KEY > ./key; chmod 0600 ./key"
        sh "git clone --branch ${params.repo_https} ${params.repo_https} ./target_gem"
      }
    }
    stage('Build Gem') {
      steps {
        sh 'gem build ./target_gem/*.gemspec'
      }
    }
    stage('Upload Gem') {
      steps {
        sh 'scp *.gem geminabox:/var/geminabox-data/gems'
        sh 'ssh root@geminabox gem generate_index -d /var/geminabox-data/'
      }
    }
  }
  parameters {
    string(name: 'repo_https', description: 'The HTTPS url of the repository for which you would like to build a gem', defaultValue: 'https://github.com/SasSwart/cd')
    string(name: 'ref', description: 'Tag, Branch or commit', defaultValue: 'master')
  }
}