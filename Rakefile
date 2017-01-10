require 'html-proofer'

task :default => [:test]

task :test => :build do
  HTMLProofer.check_directory('./_site',
                              empty_alt_ignore: true,
                              disable_external: true).run
end

task :build do
  sh 'bundle exec jekyll build'
end

task :clean do
  sh 'bundle exec jekyll clean'
end
