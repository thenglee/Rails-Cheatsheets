# Rails App with Git & Heroku 
### (includes local PostgreSQL database configuration)

Below are the steps to create a new rails app using Git as version source control and Heroku for production deployment. This guide also includes optional steps to configure PostgreSQL database locally for your development and test environments.

[Based on Ruby v2.2.2, Rails v4.2.2, Git v2.5.0, Heroku Toolbelt v3.41.4, PostgreSQL v9.4.4]

[last modified: 31.08.2015]

#### Step 1: Create new rails app

* Create a new rails project:
  ```
  $ rails new sample_app
  ```
* Go to the project root directory:
  ```
  $ cd sample_app
  ```
#### Step 2: Setup Git repository

* Create a new repository
  ```
  $ git init
  ```
* Add **_secrets.yml_** to **_.gitignore_** 
  * Go to your **_.gitignore_** file and add in the following line
     ```
     /config/secrets.yml
     ```
* Check files to be added to *staging area*
    ```
    $ git status
    ```
* Add all files to *staging area*
   ```
   $ git add -A
   ```
* Verify files in the *staging area*
   ```
   $ git status
   ```
* Commit files to git repository
  ```
  $ git commit -m "Initial commit"
  ```

#### Step 3: Setup Heroku
**Note**: Setting up PostgreSQL database locally is optional and you can proceed with the following steps below if you want to continue to use the local default database (SQLite3) provided by Rails.

However, if you want to use **PostgreSQL** database in your development and test environments, make sure you already have PostgreSQL installed before proceeding with the steps below. Refer to this [guide](http://www.gotealeaf.com/blog/how-to-install-postgresql-on-a-mac) on how to install PostgreSQL on Mac.



1. Update the **_Gemfile_**
    * Update your **_Gemfile_** based on the **database** you are using for your **development and test environments**:
      * **SQLite3 database (Rails default)**
        ```
        group :development, :test do
          ...other gems...
          gem 'sqlite3'
        end
        
        # Add pg gem in the production environment 
        group :production do 
          gem 'pg'
        end
        ```
      * **PostgreSQL database**
        ```
        # Use PostgreSQL as the database for ActiveRecord
        gem 'pg', ~> 0.18.2
        ```
2. Serve static assets
   * Go to **_config/application.rb_** and add this line: 
     ```
     config.server_static_assets = true
     ```
   * Alternatively, you can include the *rails_12factor* gem in your **_Gemfile_**:
     ```
     gem 'rails_12factor', group: :production
     ```
3. Run `$ bundle install`
4. Configure and create your local database (for local PostgreSQL database only)
    
    **Note**: Skip this step and go to step 5 if you are **not** using PostgreSQL in your development and test environments.
   * Update **_config/database.yml_** to the following:
     ```
     #   Ensure the PostgreSQL gem is defined in your Gemfile
     #   gem 'pg'
     
     development:
       adapter: postgresql
       encoding: unicode
       database: sample_app_dev
       pool: 5
       username: your_username
       password:

     # Warning: The database defined as "test" will be erased and
     # re-generated from your development database when you run "rake".
     # Do not set this db to the same as development or production

     test:
       adapter: postgresql
       encoding: unicode
       database: sample_app_test
       pool: 5
       username: your_username
       password:

     production:
       adapter: postgresql
       host: localhost
       port: 5432
       database: sample_app_production
     ```
     
   * Create the local database
      ```
      $ rake db:create
      ```
5. Add SSH key to Heroku
    
   **Info**: SSH keys only need to be added once to your Heroku account for your local machine. You can check if the SHH keys has already been added by running `$ heroku keys`
   * Login to Heroku
      ```
      $ heroku login
      ```
   * Add SSH keys
      ```
      $ heroku keys:add
      ```
6. Create your app on Heroku
    ```
    $ heroku apps:create sample_app
    ```
7. Deploy to Heroku
    ```
    $ git push heroku master
    ```
8. Run data migrations (if any) on Heroku
    ```
    $ heroku run rake db:migrate --app sample_app
    ```
9. See your deployed app in production
   * `$ heroku open`
   * Alternatively, you can visit the address you saw when you ran `heroku apps:create sample_app`
