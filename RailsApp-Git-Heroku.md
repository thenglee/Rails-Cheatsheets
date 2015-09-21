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
  * Go to your **_.gitignore_** file and add in the following line:

     ```
     /config/secrets.yml
     ```
* Check files to be added to *staging area*:

    ```
    $ git status
    ```
* Add all files to *staging area*:

   ```
   $ git add -A
   ```
* Verify files in the *staging area*:

   ```
   $ git status
   ```
* Commit files to git repository:

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
        
        *Replace these lines*
        ```
        # Use sqlite3 as the database for Active Record
        gem 'sqlite3'
        ```
        *with*

        ```
        # Use PostgreSQL as the database for Active Record 
        gem 'pg', '~>0.18.2'
        ```
2. Add *rails_12factor* gem
  
   * Include the *rails_12factor* gem in your **_Gemfile_**:

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
   * Login to Heroku:

      ```
      $ heroku login
      ```
   * Add SSH keys:

      ```
      $ heroku keys:add
      ```
6. Create your app on Heroku

    ```
    $ heroku apps:create sample-app
    ```
7. Setup SECRET_KEY_BASE environment variable in Heroku
   * Go to **_config/environments/production.rb_** and add the line:


     ```
     config.secret_key_base = ENV["SECRET_KEY_BASE"]
     ```
       
    This tells your app to set the secret_key_base using the environment variable instead of looking for it in *config/secrets.yml*

   * Generate the secret key in your console for use later in the next step:

     ```
     $ rake secret
     ```
   * Setup config vars for your app

     ```
     $ heroku config:set SECRET_KEY_BASE=...secret key here...
     ```
8. Add files to staging area and issue commit to git:

   ```
   $ git add -A
   $ git commit -m "Setup app for Heroku"
   ```

9. Deploy to Heroku

    ```
    $ git push heroku master
    ```
10. Run data migrations (if any) on Heroku

    ```
    $ heroku run rake db:migrate --app sample-app
    ```
11. See your deployed app in production
   * `$ heroku open`
   * Alternatively, you can visit the address you saw when you ran `heroku apps:create sample-app`

----------------------------------------------------------
#### References
- [JumpstartLab - Contact Manager](http://tutorials.jumpstartlab.com/projects/contact_manager.html)
- [Michael Hartl Rails Tutorial Chapter 1](https://www.railstutorial.org/book/beginning#cha-beginning)
- [Tealeaf Academy Blog - How to Install PostgreSQL for Mac OS X](http://www.gotealeaf.com/blog/how-to-install-postgresql-on-a-mac)
- [Heroku Rails 4 Asset Pipeline - Serve assets](https://devcenter.heroku.com/articles/rails-4-asset-pipeline#serve-assets)
- [Jay Lee - Heroku: Missing secret_token and secret_key_base for ‘production’](http://jaylee.com/heroku-missing-secret_toekn-and-secret_key_base-for-production/)
- [Stackoverflow - How to solve error “Missing 'secret_key_base'...](http://stackoverflow.com/questions/23180650/how-to-solve-error-missing-secret-key-base-for-production-environment-on-h)
- [Heroku - Setting up config vars for a deployed application](https://devcenter.heroku.com/articles/config-vars#setting-up-config-vars-for-a-deployed-application)

