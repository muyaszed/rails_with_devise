# rails_with_devise
Step by step how to implement user management in rails using devise. For this app, we will create a simple blog to demonstrate devise system.


##Step 1
Create a new rails application, in your command prompt
```
$ rails new <your_app_name>

```

##Step 2
When everything is generated, go to the new app folder and open up the "Gemfile" on your favorite editor
Include the [Devise](https://github.com/plataformatec/devise) gem inside this file
```ruby
	gem 'devise'

```
and then run bundle on your terminal

```
$ bundle install

```

##Step 3
You need to initialize devise by running the command below in your terminal
```
	$ rails generate devise:install
```

You will see some instruction like below
```
	1. Ensure you have defined default url options in your environments files. Here
     is an example of default_url_options appropriate for a development environment
     in config/environments/development.rb:

       config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }

     In production, :host should be set to the actual host of your application.

  2. Ensure you have defined root_url to *something* in your config/routes.rb.
     For example:

       root to: "home#index"

  3. Ensure you have flash messages in app/views/layouts/application.html.erb.
     For example:

       <p class="notice"><%= notice %></p>
       <p class="alert"><%= alert %></p>

  4. If you are deploying on Heroku with Rails 3.2 only, you may want to set:

       config.assets.initialize_on_precompile = false

     On config/application.rb forcing your application to not access the DB
     or load models when precompiling your assets.

  5. You can copy Devise views (for customization) to your app by running:

       rails g devise:views

```
You can skip number 1 and 4 for now, as we are not setting up [action mailer](http://guides.rubyonrails.org/action_mailer_basics.html) and will not deploy to heroku for this app.

You can proceed with number 2, 3 and 5 assume that you have some knowledge on the basic of Ruby on Rails.

##Step 4
You need to attach devise to a model. Let say you would like to use User as the model so we can do this by typing the command below in your terminal
```
	$ rails generate devise USER
```
If you already have an existing User model, devise will overwrite it. This command will generate all necessary file for devise to work including a migration file.

Don't perform a migration yet, as you need to modify this file later.

##Step 5
In a blog, a user could post something with a title and content. You can generate the model, controller and view accordingly, for know you can use the scaffold method. Run the command below to create everything.
```
	$ rails generate scaffold Post title:string content:text
```

