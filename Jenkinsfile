pipeline {
  agent any
  stages {
    stage('Pull Repository') {
      steps {
        cleanWs()
        checkout([$class: 'GitSCM', branches: [[name: '**']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'cd-deploy-key', name: 'origin', refspec: "+refs/tags/${params.ref}:refs/remotes/origin/tags${params.ref}", url: params.repo]]])
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