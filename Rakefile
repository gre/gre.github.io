require 'rubygems'
require 'jekyll'
require 'tmpdir'
require 'shellwords'

# Change your GitHub reponame
GITHUB_REPONAME = "gre/gre.github.io"

task :default => [:generate]

desc "Generate stylesheets from LESS"
task :style do
  system "lessc -x style/main.less > style/main.css"
end

desc "Generate blog files"
task :generate => [:style] do
  Jekyll::Site.new(Jekyll.configuration({
    "source"      => ".",
    "destination" => "_site"
  })).process
end


desc "Generate and publish blog to gh-pages"
task :publish => [:generate] do
  Dir.mktmpdir do |tmp|
    cp_r "_site/.", tmp
    Dir.chdir tmp
    system "touch .nojekyll"
    system "git init"
    system "git add ."
    message = "Site updated at #{Time.now.utc}"
    system "git commit -m #{message.shellescape}"
    system "git remote add origin git@github.com:#{GITHUB_REPONAME}.git"
    system "git push origin master --force"
  end
end
