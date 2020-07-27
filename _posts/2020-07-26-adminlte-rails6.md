---
layout: post
title:  "Integrate AndminLTE with Ruby On Rails 6"
date:   2020-07-26 20:58:50 -0500
categories: tutorial rails
tags: rails ruby adminlte
---

In this post, I want to share with you a little guide that will show you how to integrate the AdminLTE template with Ruby on Rails 6. It's just a guide that I made for myself, but I want to share it with you.

## Requirements

- Ruby (I use ruby 2.7.1p83)
- Ruby on Rails (I use Rails 6.0.3.2)
- [NodeJS](https://nodejs.org/es/).
- [Yarn](https://yarnpkg.com/).

**Note**: I advise you to install ruby using a version manager like [rvm](https://rvm.io/rvm/install) or [rbenv](https://github.com/rbenv/rbenv). Also, I recommend the [GoRails Tutorial](https://gorails.com/setup/windows/10) that will guide you throughout the process.

## Step 1: Setup Project

We are going to install the tools we need, before initializing the rails project.

```bash
# Using npm to install yarn
# You may have to run this command using sudo on Linux
npm install --global yarn

# Installing ruby using rvm. However, you can use the tool that you prefer
rvm install 2.7

# List ruby versions installed
rvm list

# Use a specific ruby version
rvm use 2.7

# Install bundler
gem install bundler

# Install Ruby on Rails v6
gem install rails -v 6.0.3.2
```

You can check the installed tools with the following commands.

```bash
# Checking yarn version
yarn -v

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

AdminLTE depends on JQuery and Bootstrap, so we have to install them. Rails 6 comes with webpacker, so we can install these dependencies using `npm` or `yarn`.

```bash
# For AdminLTE 2.4
yarn add bootstrap@^3.3.7 jquery@^3.2 popper.js@^1.16.1

# For AdminLTE 3
yarn add bootstrap jquery popper.js
```

Configure the Jquery and Popper aliases in the `config/webpack/environment.js` file.

```javascript
const {environment} = require('@rails/webpacker')

const webpack = require('webpack')

environment.plugins.append('Provide',
    new webpack.ProvidePlugin({
        $: 'jquery',
        jQuery: 'jquery',
        Popper: ['popper.js', 'default']
    })
)

module.exports = environment
```

Import bootstrap into the `app/javascript/packs/application.js` file.

```javascript
// app/javascript/packs/application.js
// ....
import 'bootstrap';

document.addEventListener("turbolinks:load", () => {
  $('[data-toggle="tooltip"]').tooltip()
});
// ....
```

Add the following to the `config/webpacker.yml` file.

```yaml
resolved_paths: ['app/assets']
```

Create the directory `app/javascript/stylesheets/` to put the style files here. And to use webpack to compile these assests.

```bash
mkdir app/javascript/stylesheets
```

Create the `app/javascript/stylesheets/application.scss` file.

```bash
touch app/javascript/stylesheets/application.scss
```

**For AdminLTE 2.4**

Import bootstrap css files into the `app/javascript/packs/application.js` file. I had some issues importing bootstrap into the `app/javascript/stylesheets/application.scss` file; However, since we are using webpack, we can import css files into a js file. 

**Note:** You can also use sprockets for this step to avoid importing css files into js files.

```javascript
import 'bootstrap/dist/css/bootstrap.css';
import 'bootstrap/dist/css/bootstrap-theme';
import '../stylesheets/application'; // This file will contain your custom CSS
```

**For AdminLTE 3.0**

Import bootstrap into the `app/javascript/stylesheets/application.scss` file.

```scss
// app/javascript/stylesheets/application.scss
@import "bootstrap";
```

Import the styles into the `app/javascript/packs/application.js` file.

```javascript
// app/javascript/packs/application.js
// ......
import '../stylesheets/application'; // This file will contain your custom CSS
```

Now, you can run the project again.

```bash
rails server
```

## Step 4: Install AdminLTE template

Install the adminlte package.

```bash
# AdminLTE 2.4 package
yarn add admin-lte@^2.4.18

# AdminLTE 3.0 package
yarn add admin-lte@^3.0
```

Import the AdminLTE scripts into the `app/javascript/packs/application.js` file.

```javascript
// app/javascript/packs/application.js
....
require('admin-lte');
```

**For AdminLTE 2.4**

Import the AdminLTE stylesheets into the `app/javascript/packs/application.js` file.

```javascript
/* For AdminLTE 2.4 */
import "admin-lte/dist/css/AdminLTE.css";
import "admin-lte/dist/css/skins/_all-skins.css";
```

**For AdminLTE 3.0**

Import the AdminLTE stylesheets into the `app/javascript/stylesheets/application.scss` file.

```scss
/* For AdminLTE 3 */
@import "admin-lte";
```

We also need to install `font-awesome`.

```bash
# For AdminLTE 2.4
yarn add font-awesome@4.7.0

# For AdminLTE 3
yarn add @fortawesome/fontawesome-free
```

**For AdminLTE 2.4**

Import fontawesome styles into the `app/javascript/packs/application.js` file.

```javascript
/* For AdminLTE 2.4 */
import "font-awesome/css/font-awesome.css";
```

**For AdminLTE 3.0**

Import fontawesome styles into `app/javascript/stylesheets/application.scss`

```scss
/* For AdminLTE 3 */
@import '@fortawesome/fontawesome-free';
```

For AdminLTE 3, import fontawesome scripts into `app/javascript/packs/application.js`

```javascript
// ....
import "@fortawesome/fontawesome-free/js/all";
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

    <%= stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
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

    <%= stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
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

### Structuring views

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

We will use the `starter.html` page to structure our views. You can find the starter page in `node_modules/admin-lte/starter.html`. 

- Copy the `class="main-header"` section into the `app/views/base/_header.html.erb` file.
- Copy the `class="main-sidebar ..."` section into the `app/views/base/_sidebar.html.erb` file.
- Copy the  `class="main-footer"` section into the `app/views/base/_footer.html.erb` file.
- Copy the `class="control-sidebar ..."` and `class="control-sidebar-bg"` sections into the `app/views/base/_control_sidebar.html.erb`.

Finally, Run the project again.

```bash
rails server
```

## Final Words
You can find the code of this guide [here](https://gitlab.com/brayvasq/adminlte-rails-template). And, you can find the Rails 5 version in my blog, [Integrate Ruby on Rails 5 and AdminLTE](https://brayvasq.github.io/tutorial/rails/2020/07/26/adminlte-rails5.html).
