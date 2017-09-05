Dir.chdir 'src' # Go into source dir.

task :build do
  require 'tty/command'

  cmd = TTY::Command.new
  cmd.run 'apt update -y'
  cmd.run 'apt install -y maven'
  cmd.run 'mvn clean install -DskipTests=true -U'
end

task :publish do
  require "#{Dir.home}/tooling/ci-tooling/lib/jenkins"
  # require "/home/me/src/git/pangea-tooling/ci-tooling/lib/jenkins"
  require 'faraday'

  config = AutoConfigJenkinsClient.config
  connection = Faraday.new('https://build.neon.kde.org') do |c|
    c.request :multipart
    c.request :url_encoded
    c.adapter :excon
    c.basic_auth(config.fetch(:username), config.fetch(:password))
  end

  response = connection.get('/crumbIssuer/api/json').body
  crumb = JSON.parse(response, symbolize_names: true)
  headers = { crumb.fetch(:crumbRequestField) => crumb.fetch(:crumb) }

  v = "#{Dir.pwd}/target/digitalocean-plugin.hpi"
  payload = {}
  payload[:hpi] = Faraday::UploadIO.new(v, 'application/binary')
  p connection.post('/pluginManager/uploadPlugin', payload, headers)
end
