// env.MAVEN_OPTS = "-Dmaven.repo.local=/workspace"


node('master') {
  stage('clone') {
    git branch: 'neon', url: 'https://github.com/apachelogger/digitalocean-plugin'
  }
  stage('build') {
    sh '~/tooling/nci/contain.rb rake build'
  }
  stage('publish') {
    sh 'rake publish'
  }
}
