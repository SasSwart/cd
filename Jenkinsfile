pipeline {
  agent any
  stages {
    stage('Pull Repository') {
      steps {
        cleanWs()
        git branch: params.ref, credentialsId: 'cd-deploy-key', url: params.repo
      }
    }
    stage('Build Gem') {
      steps {
        sh 'gem build ./target_gem/*.gemspec'
      }
    }
    stage('Upload Gem') {
      steps {
        sh 'scp ./target_gem/*.gem geminabox:/var/geminabox-data/gems'
        sh 'ssh root@geminabox gem generate_index -d /var/geminabox-data/'
      }
    }
    stage('Cleanup') {
      steps {
        cleanWs()
      }
    }
  }
  parameters {
    string(name: 'repo', description: 'The HTTPS url of the repository for which you would like to build a gem', defaultValue: 'https://github.com/SasSwart/cd')
    string(name: 'ref', description: 'Tag, Branch or commit', defaultValue: 'master')
  }
}