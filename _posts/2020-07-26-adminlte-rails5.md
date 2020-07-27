---
layout: post
title:  "Integrate AndminLTE with Ruby On Rails 5"
date:   2020-07-26 20:55:50 -0500
categories: tutorial rails
tags: rails ruby adminlte
---

In this post, I want to share with you a little guide that will show you how to integrate the AdminLTE template with Ruby on Rails 5. It's just a guide that I made for myself, but I want to share it with you.

## Requirements

- Ruby (I use ruby 2.6.5p114)
- Ruby on Rails (I use Rails 5.2.0)
- [NodeJS](https://nodejs.org/es/).

**Note**: I advise you to install ruby using a version manager like [rvm](https://rvm.io/rvm/install) or [rbenv](https://github.com/rbenv/rbenv). Also, I recommend the [GoRails Tutorial](https://gorails.com/setup/windows/10) that will guide you throughout the process.

## Step 1: Setup Project

We are going to install the tools we need, before initializing the rails project.

```bash
# Installing ruby using rvm. However, you can use the tool that you prefer
rvm install 2.6

# List ruby versions installed
rvm list

# Use a specific ruby version
rvm use 2.6

# Install bundler
gem install bundler

# Install Ruby on Rails v6
gem install rails -v 5.2
```

You can check the installed tools with the following commands.

```bash
# Checking ruby version
ruby -v

# Checking bundler version
bundle -v

# Checking bundle version version
rails -v
```

Once you checked that you have the tools installed. Let's move to create the rails project.

```bash
rails new adminlte-rails-template
```

## Step 2: Setup Controller

Here, we are going to create a controller in which we will place the AdminLTE template.

```bash
# First you have to move to the app directory
cd adminlte-rails-template/

# Generate a controller only with the action index
rails generate controller home index
```

Set the default Url in the `config/routes.rb` file.

```ruby
# config/routes.rb
root 'home#index'
```

Run the project.

```bash
rails server
```

## Step 3: Setup JQuery and Bootstrap

AdminLTE depends on JQuery and Bootstrap, so we have to install them. Add the following gems to the `Gemfile`.

```ruby
# Gemfile
# ....
# AdminLTE 2.4
gem 'bootstrap-sass'
gem 'font-awesome-rails'
gem "jquery-rails"
gem 'popper_js', '~> 1.12.9'

# AdminLTE 3
gem 'bootstrap', '~> 4.3.1'
gem 'font-awesome-rails'
gem "jquery-rails"
gem 'popper_js', '~> 1.14'
```

Install dependencies

```bash
bundle install
```

Import scripts into the `app/assets/javascript/application.js` file.

```javascript
//= require jquery
//= require jquery_ujs
//= require popper
//= require bootstrap
```

Rename `app/assets/stylesheets/application.css` to `app/assets/stylesheets/application.scss` and import styles.

```scss
 @import "bootstrap";
 @import "font-awesome";
```

Now, you can run the project again.

```bash
rails server
```

## Step 4: Install AdminLTE template

Download template

```bash
# AdminLTE 2.4 package
wget https://github.com/ColorlibHQ/AdminLTE/archive/v2.4.18.zip

# Extract files
unzip v2.4.18.zip

# AdminLTE 3.0 package
wget https://github.com/ColorlibHQ/AdminLTE/archive/v3.0.5.zip

# Extract files
unzip v3.0.5.zip
```

**For AdminLTE 2.4**

- Create the folder `app/assets/javascripts/adminlte`. 

- Copy the `AdminLTE-2.4.18/dist/js/adminlte.js` file into  the `app/assets/javascripts/adminlte` folder.

- Create the folder  `app/assets/stylesheets/adminlte`. 
- Copy the `AdminLTE-2.4.18/dist/css/AdminLTE.css` into the`app/assets/stylesheets/adminlte`  folder.
- Copy the `AdminLTE-2.4.18/dist/css/skins/_all-skins.css` into the`app/assets/stylesheets/adminlte ` folder.

**For AdminLTE 3.0**

- Create folder `app/assets/javascripts/adminlte`. 
- Copy the `AdminLTE-3.0.5/dist/js/adminlte.js` file into  the `app/assets/javascripts/adminlte` folder.
-  Copy the `AdminLTE-3.0.5/dist/js/adminlte.js.map` file into  the `app/assets/javascripts/adminlte` folder.
- Create the folder  `app/assets/stylesheets/adminlte`. 
- Copy `AdminLTE-3.0.5/dist/css/adminlte.css` into the`app/assets/stylesheets/adminlte`  folder.
- Copy `AdminLTE-3.0.5/dist/css/adminlte.css.map` into the`app/assets/stylesheets/adminlte`  folder.

### Import adminlte scripts and styles

Import adminlte scripts in the `app/assets/javascript/application.js` file.

```javascript
//= require adminlte/adminlte
```

Import the AdminLTE stylesheets into the `app/assets/stylesheets/application.scss` file.

```scss
/* For AdminLTE 2.4 */
@import "adminlte/AdminLTE.css";
@import "adminlte/_all-skins.css";

/* For AdminLTE 3 */
@import "adminlte/adminlte.css";
```

## Step 5: Setup Layout

Change the `app/views/layouts/application.html.erb` file content.

**For AdminLTE 2.4**

```erb
<!DOCTYPE html>
<html>
  <head>
    <title>Rails6 AdminLTE 2.4</title>
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>

    <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>
  </head>

  <body class="hold-transition skin-blue sidebar-mini">
      <div class="wrapper">
        <%= render "base/header" %>
        <%= render "base/sidebar" %>

        <div class="content-wrapper">
          <section class="content-header">
            <h1>
              Page Header
              <small>Optional description</small>
            </h1>
            <ol class="breadcrumb">
              <li><a href="#"><i class="fa fa-dashboard"></i> Level</a></li>
              <li class="active">Here</li>
            </ol>
          </section>

          <section class="content container-fluid">
            <%= yield %>
          </section>
        </div>

        <%= render "base/footer" %>
        <%= render "base/control_sidebar" %>
      <div>
  </body>
</html>
```

**For AdminLTE 3**

```erb
<!DOCTYPE html>
<html>
  <head>
    <title> Rails6 AdminLTE 3.0</title>
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>

    <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>
  </head>

  <body data-scrollbar-auto-hide="n" class="hold-transition sidebar-mini layout-fixed">
      <div class="wrapper">
        <%= render "base/header" %>
        <%= render "base/sidebar" %>

        <div class="content-wrapper">
          <div class="content-header">
            <div class="container-fluid">
              <div class="row mb-2">
                <div class="col-sm-6">
                  <h1 class="m-0 text-dark">Starter Page</h1>
                </div><!-- /.col -->
                <div class="col-sm-6">
                  <ol class="breadcrumb float-sm-right">
                    <li class="breadcrumb-item"><a href="#">Home</a></li>
                    <li class="breadcrumb-item active">Starter Page</li>
                  </ol>
                </div><!-- /.col -->
              </div><!-- /.row -->
            </div><!-- /.container-fluid -->
          </div>

          <div class="content container-fluid">
            <div class="container-fluid">
              <%= yield %>
            </div>
          </div>
        </div>

        <%= render "base/footer" %>
        <%= render "base/control_sidebar" %>
      <div>
  </body>
</html>
```

Create the folder `app/views/base`.

```bash
mkdir app/views/base
```

In the `app/views/base`, we are going to create a few files.

 ```bash
# AdminLTE header layout
touch app/views/base/_header.html.erb

# AdminLTE sidebar layout
touch app/views/base/_sidebar.html.erb

# AdminLTE footer layout
touch app/views/base/_footer.html.erb

# AdminLTE control-sidebar layour
touch app/views/base/_control_sidebar.html.erb
 ```

We will use the `starter.html` page to structure our views. You can find the starter page in the `AdminLTE-x.x.x/starter.html` folder (created when you extract the files using `unzip`). 

- Copy the `class="main-header"` section into the `app/views/base/_header.html.erb` file.
- Copy the `class="main-sidebar ..."` section into the `app/views/base/_sidebar.html.erb` file.
- Copy the  `class="main-footer"` section into the `app/views/base/_footer.html.erb` file.
- Copy the `class="control-sidebar ..."` and `class="control-sidebar-bg"` sections into the `app/views/base/_control_sidebar.html.erb`.

Finally, Run the project again.

```bash
rails server
```

## Final Words
You can find the code of this guide [here](https://gitlab.com/brayvasq/adminlte-rails-template/-/tree/v.2). And, you can find the Rails 6 version in my blog, [Integrate Ruby on Rails 6 and AdminLTE](https://brayvasq.github.io/tutorial/rails/2020/07/26/adminlte-rails6.html).

