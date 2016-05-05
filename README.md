# rails_with_devise
Step by step how to implement user management in rails using devise


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

