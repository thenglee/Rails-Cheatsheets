### Useful commands
- `rvm list`: show currently installed rubies, interactive output
- `rvm use` : setup current shell to use a specific ruby version
- `which ruby`: shows the full path of the current ruby version
- `less /<directory>`: view the contents of a text file one screen at a time
- `gem list`: show the gems you have installed locally
- `gem list | grep pg`: search and show the gems containing a match to the given strings or words, in this case 'pg'
- `rake secret`: give you a pseudo-random key to use for your session secret
- `rake db:create`: create the database locally
- `bundle install`: check the gems in the Gemfile and install the missing gems in the *current* ruby version 


### Git commands
- `git remote`: list the set of tracked repositories
- `git remote add heroku https://git.heroku.com/sample-app.git`: adds a remote named <name> for the repository at <url>
- `git remote show heroku`: shows information about the remote
- `git add .` or `git add -A`: Add all the files in your current directory
- `git add -u .`: Add **only** those files already being tracked (Often used for staging tracked files that are deleted from disk)
- `git commit --amend`: amend commit message
- `git reset --soft HEAD^`: restore back to the state before the last commit, **keeping the changes you made in the last commit back into staging area**
- `git reset --hard HEAD^`: restore back to the state before the last commit, **removing all the changes you made in the last commit**
- `git push -f`: replace upstream history in the remote (usually used after `git reset --hard HEAD^`)


### Heroku commands
- `heroku create`: creates a new application on Heroku
- `heroku login`: login to heroku
- `heroku keys`: shows the added SSH keys, if any
- `heroku keys:add`: add SSH keys to heroku
- `heroku apps`: list all your heroku apps
- `heroku logs --tail --app sample-app`: show continually stream logs on the heroku server 
- `heroku run console --app sample-app`: run heroku console
- `heroku run rake db:migrate`: run data migrations in heroku
