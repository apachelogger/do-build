// env.MAVEN_OPTS = "-Dmaven.repo.local=/workspace"
env.DIST = 'xenial'
env.TYPE = 'user'
env.PWD_BIND = '/workspace'

node('master') {
  stage('clone') {
    checkout scm
    sh 'ls -lah'
    git branch: 'neon', url: 'https://github.com/apachelogger/digitalocean-plugin'
    sh 'ls -lah'
  }
  stage('build') {
    sh 'ls -lah'
    sh '~/tooling/nci/contain.rb rake build'
  }
  stage('publish') {
    sh 'rake publish'
  }
}
