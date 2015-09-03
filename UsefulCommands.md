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
- `git remote add heroku https://git.heroku.com/sample-app.git`: Adds a remote named <name> for the repository at <url>
-  `git remote show heroku`: Shows information about the remote

### Heroku commands
- `heroku create`: creates a new application on Heroku
- `heroku login`: login to heroku
- `heroku keys`: shows the added SSH keys, if any
- `heroku keys:add`: add SSH keys to heroku
- `heroku apps`: list all your heroku apps
- `heroku logs --tail --app sample-app`: show continually stream logs on the heroku server 
- `heroku run console --app sample-app`: run heroku console
- `heroku run rake db:migrate`: run data migrations in heroku

