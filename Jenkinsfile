// env.MAVEN_OPTS = "-Dmaven.repo.local=/workspace"
env.DIST = 'xenial'
env.TYPE = 'user'
env.PWD_BIND = '/workspace'

node('master') {
  wrap([$class: 'AnsiColorBuildWrapper', colorMapName: 'xterm']) {
    stage('clone') {
      checkout scm
      sh '[ -d src ] || mkdir src'
      dir('src') {
        git branch: 'neon', url: 'https://github.com/apachelogger/digitalocean-plugin'
      }
    }
    stage('build') {
      sh 'ls -lah'

      sh '~/tooling/nci/contain.rb rake build'
    }
    stage('publish') {
      sh 'rake publish'
    }
  }
}
