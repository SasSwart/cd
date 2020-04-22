pipeline {
  agent any
  stages {
    stage('Pull Repository') {
      steps {
        cleanWs()
        sh 'git clone --branch ${params.ref} ${params.repo_https} .'
      }
    }
    stage('Build Gem') {
      steps {
        sh 'gem build *.gemspec'
      }
    }
    stage('Upload Gem') {
      steps {
        sh 'scp *.gem geminabox:/var/geminabox-data/gems'
        sh 'ssh root@geminabox gem generate_index -d /var/geminabox-data/'
      }
    }
    stage('Cleanup') {
      cleanWs()
    }
  }
  parameters {
    string(name: 'repo_https', description: 'The HTTPS url of the repository for which you would like to build a gem', defaultValue: 'https://github.com/SasSwart/cd')
    string(name: 'ref', description: 'The branch or tag to clone for build', defaultValue: 'master')
  }
}