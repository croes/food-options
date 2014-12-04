require 'fileutils'

task :init_local do
  Dir.mkdir 'build' unless File.directory? 'build'
end

task :generate do
  system 'bundle exec middleman build --clean'
end

desc 'Generate site'
task :build => [:init_local, :generate]

task :publish do
  Dir.chdir 'build'
  system "git config user.name '#{ENV['GIT_NAME']}'"
  system "git config user.email '#{ENV['GIT_EMAIL']}'"
  system 'git config credential.helper "store --file=.git/credentials"'
  File.open('.git/credentials', 'w') do |f|
    f.write("https://#{ENV['GH_TOKEN']}:@github.com")
  end
  Dir.chdir '..'
  system 'bundle exec middleman deploy'
  File.delete 'build/.git/credentials'
end

desc 'Generate site from Travis CI and publish site to GitHub Pages'
task :travis => [:build, :publish]

unless ENV['TRAVIS_PULL_REQUEST'].to_s.to_i > 0
# Only publish when we're not building to validate a pull request
  task :travis => [:publish]
end
