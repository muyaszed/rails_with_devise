# rails_with_devise
This is my own note which is a step by step on how to implement user management in rails using devise. For this app, we will create a simple blog to demonstrate devise system. Anybody is free to use this note for learning and non-commercial purposes.


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

##Step 6
By default devise does not include name field for user to register. This step is optional if you like name to be include when user regsiter thier profile. Open and edit the migration file created when we generated the User model using devise on Step 4. As you can see, there is no 'name' field in the User table, so we will include one.
```
	class DeviseCreateUsers < ActiveRecord::Migration
  def change
    create_table :users do |t|

	  #add name on user registration
	  t.string :name #<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< new line added

      ## Database authenticatable
      t.string :email,              null: false, default: ""
      t.string :encrypted_password, null: false, default: ""

      ## Recoverable
      t.string   :reset_password_token
      t.datetime :reset_password_sent_at

      ## Rememberable
      t.datetime :remember_created_at

      ## Trackable
      t.integer  :sign_in_count, default: 0, null: false
      t.datetime :current_sign_in_at
      t.datetime :last_sign_in_at
      t.string   :current_sign_in_ip
      t.string   :last_sign_in_ip

      ## Confirmable
      # t.string   :confirmation_token
      # t.datetime :confirmed_at
      # t.datetime :confirmation_sent_at
      # t.string   :unconfirmed_email # Only if using reconfirmable

      ## Lockable
      # t.integer  :failed_attempts, default: 0, null: false # Only if lock strategy is :failed_attempts
      # t.string   :unlock_token # Only if unlock strategy is :email or :both
      # t.datetime :locked_at


      t.timestamps null: false
    end

    add_index :users, :email,                unique: true
    add_index :users, :reset_password_token, unique: true
    # add_index :users, :confirmation_token,   unique: true
    # add_index :users, :unlock_token,         unique: true
  end
end

	
```
In order for this to work, you need to overwrite devise controller, to do this you need to create your own controller called 'registrations'
```
	$ rails g controller registrations
```

Then open up the file to edit.
First, since you want to overwrite devise function, you need to inherit devise controller. Change the class like below
```
	class RegistrationsController < Devise::RegistrationsController
	end
```

Next you need to add methods that will handle all the user params including the new field 'name'. You can add these methods below
```
	private

	def sign_up_params
		params.require(:user).permit(:name, :email, :password, :password_confirmation)
	end

	def account_update_params
		params.require(:user).permit(:name, :email, :password, :password_confirmation, :current_password)
	end
``` 

so the controller file will overall look like this
```
	class RegistrationsController < Devise::RegistrationsController

		private

		def sign_up_params
			params.require(:user).permit(:name, :email, :password, :password_confirmation)
		end

		def account_update_params
			params.require(:user).permit(:name, :email, :password, :password_confirmation, :current_password)
		end
	end

```

Finally you will need to update the registration form in views. Open up the new and edit views from views/devise/registrations and add the field below

```html(rails)
	<div class="field">
    <%= f.label :name %><br />
    <%= f.text_field :name, autofocus: true %>
  </div>
```

##Step 7
You need to add some links to your layout so user can login, edit profile and log out. Add the code bwlow to your application.html.erb just right after the <body> opening tag.
```html(rails)
<body>	
	<header>
		<%if user_signed_in?%>
			<%= link_to "Edit", edit_user_registration_path(current_user) %> | <%= link_to "Log Out", destroy_user_session_path, :method => "delete" %>
		<%else%>
			<%= link_to "Log In", new_user_session_path %>  | <%= link_to "Sign Up", new_user_registration_path %>
		<%end%>
	</header>
```
This will render edit and logout link when the user is signed in and Log in link vice versa.

##Step 8
Run the migration by using the command below on your terminal
```
	$ Rake db:migrate
```

##Step 9
Run your server and test your app


