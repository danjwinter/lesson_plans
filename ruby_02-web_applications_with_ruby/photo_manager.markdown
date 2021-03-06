---
title: Photo Manager
length: 180
tags: rails
---

We'll create an app where photos are stored. Photos can belong to either a product or an employee. 

### Goals

By the end of this tutorial, you will know/be able to:

* deploy an app to Heroku
* write model tests using RSpec
* write feature tests using RSpec and Capybara
* use branches and small commits to develop good git workflow
* model one-to-many data relationships
* use polymorphic associations to associate an object with one of several other models
* use partials and helpers to tidy up views
* implement authentication using Twitter OAuth

### Getting Started

Check that you have Rails installed. From your terminal, type: 

```
$ rails -v
Rails 4.2.5
```

This tutorial is written using Rails 4.2.5, but it should be applicable to any similar version. 

Create a new Rails project:

```
$ rails new photo_manager -d postgresql --skip-test-unit --skip-turbolinks --skip-spring
```

What do these things do?

* `-d postgresql` will give us a Rails project that already has Postgresql configured
* `--skip-test-unit` will tell rails to **not** generate the `test` directory and the default Test::Unit framework
* `--skip-turbolinks` and `--skip-spring` will generate a project without turbolinks and spring, both of which can cause some annoying behavior

### Using RSpec instead of Test::Unit

So far, we've done a lot with Minitest and Test::Unit since that's what Rails uses by default. However, we're going to use [RSpec](https://github.com/rspec/rspec-rails) for this tutorial. You should be comfortable with both frameworks. There are a few steps we'll take to use RSpec in our project. 

1) Add RSpec as a depencency in the Gemfile. The `group :development, :test...` probably already exists in the Gemfile. You'll just need to add `gem 'rspec-rails'`.

```ruby
group :development, :test do
  gem 'rspec-rails'
end
```

2) Install this dependency:

```
$ bundle install
```

(`install` is the default for Bundler, so `bundle install` and `bundle` will get you the same behavior) 

3) Run the generator to create initial RSpec setup:

```
$ rails generate rspec:install
      create  .rspec
      create  spec
      create  spec/spec_helper.rb
      create  spec/rails_helper.rb
```

This created a [`.rspec` file](https://www.relishapp.com/rspec/rspec-core/v/2-0/docs/configuration/read-command-line-configuration-options-from-files), a [`spec` directory](https://www.relishapp.com/rspec/rspec-rails/docs/directory-structure) (this is equivalent to the `test` directory if we were using Test::Unit), and [two helper files](https://relishapp.com/rspec/rspec-rails/docs/upgrade). 

### Using Unicorn instead of WEBrick

By default, Rails uses the Webrick server which is slow. Let's use Unicorn instead. In your Gemfile, uncomment this line:

```ruby
gem 'unicorn'
```

Then run `bundle`. To start up the server:

```
# Use Unicorn as the app server
$ unicorn
```

### Git Setup

Initialize your project as a Git repository:

```
$ git init
```

Then add and commit:

```
$ git add .
$ git commit -m 'initial commit'
```

### Shipping to Heroku

We want to host our Rails application on the internet using the popular [Heroku](https://www.heroku.com/) service.

If you don’t already have one, you’ll need to create a [Heroku account](https://signup.heroku.com/www-header). After creating your account download and install the [Heroku Toolbelt](https://toolbelt.heroku.com/).

Heroku requires applications to use a PostgresSQL database and not a Sqlite database. Good thing we generated our Rails project with a Postgres database! Some people may choose to use Sqlite locally when running in test and development and PostgresSQL only on Heroku. This configuration is indeed possible; however, it's important to remember that these databases have slightly different behaviors, so something that works with Sqlite may not work with Postgres, and vice versa. 

We also want to enable logging and asset delivery, so we'll add the [`rails_12factor`](https://github.com/heroku/rails_12factor) gem to our Gemfile:

```
gem 'rails_12factor', group: :production
```

Remember to `bundle`. 

Now, let's ship to Heroku! This next command will only work if you installed the Heroku Toolbelt.

```
$ heroku create
Creating adjective-noun-number... done, stack is cedar-14
https://adjective-noun-number.herokuapp.com/ | https://git.heroku.com/adjective-noun-number.git
Git remote heroku added
```

The toolbelt will ask for your username and password the first time you run the create, but after that you’ll be using an SSH key for authentication.

After running the create command, you’ll get back the URL where the app is accessible. Try loading the URL in your browser and you’ll see the generic Heroku splash screen. This is because we haven't actually pushed our code to Heroku. All we did was create the remote. 

Now push to Heroku:

```
$ git push heroku master
...
...lots and lots of output
...
remote: -----> Launching... done, v5
remote:        https://adjective-noun-number.herokuapp.com/ deployed to Heroku
```

Now navigate to your Heroku URL and **you should see an error**:

```
The page you were looking for doesn't exist.

You may have mistyped the address or the page may have moved.

If you are the application owner check the logs for more information.
```

Don't worry! Everything is fine. The reason we see this is because we haven't written any code yet. 

### Building Products

Like all good developers, we'll start by writing a feature test :) First, we'll need to set up Capybara in order to mimic an end-user. In the Gemfile:

```ruby
group :development, :test do
  gem 'capybara'
end
``` 

You may also want to add `gem 'launchy'` so that you can `save_and_open_page` for debugging purposes. 

Then `bundle`.

Next, add the dependency to the `rails_helper.rb` file:

```ruby
require 'capybara/rails'
```

Time to write the test! Make a folder for features: `mkdir spec/features` and then create a test: `touch spec/features/???_spec.rb`


### WIP

### Resources

* [Understanding Polymorphic Associations in Rails](http://www.gotealeaf.com/blog/understanding-polymorphic-associations-in-rails)
* [Testing wtih RSpec - Codeschool](http://rspec.codeschool.com/levels/1)
* [An Introduction to RSpec - Team Treehouse](http://blog.teamtreehouse.com/an-introduction-to-rspec)