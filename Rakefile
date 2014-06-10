require "rubygems"
require "bundler/setup"
#require "stringex"

## -- Misc Configs -- ##

source_dir      = "src"    # source file directory
deploy_dir      = "build"   # deploy directory (for Github pages deployment)
deploy_branch  = "master" # git deplay branch
repo_url = "git@github.com:zhenkyle/zhenkyle.github.io.git"


##############
# Deploying  #
##############

desc "deploy build directory to github pages"
multitask :deploy do
  puts "## Deploying branch to Github Pages "
  puts "## Pulling any updates from Github Pages "
  cd "#{deploy_dir}" do 
    system "git pull"
    system "git add --all ."
    puts "\n## Committing: Site updated at #{Time.now.utc}"
    message = "Site updated at #{Time.now.utc}"
    system "git commit -m \"#{message}\""
    puts "\n## Pushing generated #{deploy_dir} website"
    system "git push origin #{deploy_branch}"
    puts "\n## Github Pages deploy complete"
  end
end

desc "Set up deploy for Github Pages"
task :setup do
  rm_rf "#{deploy_dir}"
  system "git clone #{repo_url} #{deploy_dir}"
  puts "\n---\n## Now you can deploy to #{repo_url} with `rake deploy` ##"
end

desc "list tasks"
task :list do
  puts "Tasks: #{(Rake::Task.tasks - [Rake::Task[:list]]).join(', ')}"
  puts "(type rake -T for more detail)\n\n"
end
