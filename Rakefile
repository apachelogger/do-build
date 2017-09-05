Dir.chdir 'src' # Go into source dir.

task :build do
  if Dir.home.include?('jenkins') || Dir.home.include?('root')
    local = File.expand_path("#{Dir.pwd}/../.m2")
    m2 = "#{Dir.home}/.m2"
    FileUtils.mkpath(local, verbose: true) unless Dir.exist?(local)
    FileUtils.ln_s(local, m2, verbose: true)
  end

  require 'tty/command'

  cmd = TTY::Command.new
  cmd.run 'apt update -y'
  cmd.run 'apt install -y maven openjdk-8-jdk-headless'
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
  response = connection.post('/pluginManager/uploadPlugin', payload, headers)
  p response.status
  raise 'expected status 302' unless response.status == 302
end
