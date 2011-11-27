set :application, "haste-server"
set :node_file, "server.js"
set :host, "96.126.105.213"
set :repository, "git@codeplane.com:seejohnrun/haste-server.git"
set :user, "john"
set :admin_runner, "www-data"

ssh_options[:forward_agent] = true

set :scm, :git
set :branch, "production"
set :deploy_via, :remote_cache
set :deploy_to, "/var/www/haste-server"
set :keep_releases, 4

role :app, host

namespace :deploy do

  desc 'setup the deploy'
  task :setup, :roles => :app do
    run "rm -rf #{deploy_to}"
    run "mkdir -p #{deploy_to}/releases"
    run "cd #{deploy_to} && git clone #{repository} code"
  end

  desc 'execute the deploy'
  task :update, :roles => :app do
    release = "#{Time.now.to_i}"
    
    run "cd #{deploy_to}/code && git pull origin master"
    
    run "cp -Rf #{deploy_to}/code #{deploy_to}/releases/#{release}"
    run "rm #{deploy_to}/current"
    run "ln -sf #{deploy_to}/releases/#{release} #{deploy_to}/current"

    run "cd #{deploy_to}/current && npm install https://github.com/Hiverness/hashlib/tarball/compat_fix_node_0_6_X"
    run "cd #{deploy_to}/current && npm install redis"
    run "cd #{deploy_to}/current && npm install"

    run "killall node; true"
    # TODO run as different user
    # TODO use monit
    run "cd #{deploy_to}/current && `nohup npm start > /dev/null 2>&1 &`"
  end

end
