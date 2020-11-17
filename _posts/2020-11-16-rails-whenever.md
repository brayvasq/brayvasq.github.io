---
layout: post
title:  "Ruby on Rails Cron Jobs Using Whenever"
date:   2020-11-16 19:30:00 -0500
categories: tutorial rails
tags: rails ruby cron whenever
---

Hi Everyone!. In this post, I want to share with you a little guide that will show you how to use cron jobs in a Ruby on Rails application.

## Requirements

- Ruby (I use ruby 2.7.1p83)
- Ruby on Rails (I use Rails 6.0.3.4)

**Note**: I advise you to install ruby using a version manager like [rvm](https://rvm.io/rvm/install) or [rbenv](https://github.com/rbenv/rbenv). Also, I recommend the [GoRails Tutorial](https://gorails.com/setup/windows/10) that will guide you throughout the process.

## What's a Cron Job?

A cron job is a process that is scheduled to run at a certain time (every minute, day, week, or month). On Unix systems, we can run and manage these types of processes using the **cron** tool. The info of the process that will run the **cron** tool is stored in a file called **crontab**.

Examples of the use of cron jobs are the following:

- Generate reports. e.g, a report of the day payments for an e-commerce app.
- Send reminders to users. e.g send reminders of offers for an e-commerce app.
- Modify records status. e.g, cancel payments that exceed a time limit to be paid.

We can see our cron jobs using the following command

```bash
crontab -l
```

To delete user jobs we can use the following command

```bash
crontab -r
```

And to edit our crontab file, we can use the following command

```bash
crontab -e
```

The structure of a cron job is the following

```bash
.--------------- minute (0-59) 
|  .------------ hour (0-23)
|  |  .--------- day of month (1-31)
|  |  |  .------ month (1-12)
|  |  |  |  .--- day of week (0-6) (sunday=0 or 7)  
|  |  |  |  |
*  *  *  *  *  command to execute
```

An example of how to define a cron job to execute every 5 minutes

```bash
*/5 * * * *  /home/user/test.rb
```

## Setup the project

Now that we know what's a cron job, we can start to write code. First, we are going to create our rails project.

```bash
rails new whenever_example
```

## Add whenever gem

Whenever is a gem that helps us to define cron jobs in a readable way. So, we will add this gem to our `Gemfile`.

```ruby
# Gemfile

# Cron jobs
gem 'whenever', require: false
```

Install our dependencies

```bash
bundle install
```

## Create a rake task

We will use a rake task to run in our cron job. First, create a `.rake` file.

```bash
# Create the rake task
touch lib/tasks/whenever_test.rake
```

On our task, we will simply print a message.

```ruby
# lib/tasks/whenever_test.rake
desc 'Whenever rake task test'
task whenever_call: :environment do
  Rails.logger.info "Whenever task"
end
```

To see the rake tasks of our Rails application, we can use the following command.

```bash
# See all tasks
bundle exec rake --tasks

# See our task
bundle exec rake --tasks | grep whenever_call
# rake whenever_call                      # Whenever rake task test
```

We can execute our task to check that it works.

```bash
bundle exec rake whenever_call
```

In our logs file we should see the message we define.

```bash
tail log/development.log
# Whenever task
```

## Define our cron job

First, we have to setup the whenever gem.

```bash
bundle exec wheneverize .
# [add] writing `./config/schedule.rb'
# [done] wheneverized!
```

This will create the `config/schedule.rb` where we will define our cron jobs. We will create a simple task that will be executed every 2 minutes.

```ruby
# config/schedule.rb
# ....
every 2.minute do
  rake 'whenever_call'
end
```

Now, we have to update our crontab file.

```bash
whenever --update-crontab
```

If we list our cron jobs in the crontab file, we will see something like the following.

```bash
# crontab file
0,2,4,6,8,10,12,14,16,18,20,22,24,26,28,30,32,34,36,38,40,42,44,46,48,50,52,54,56,58 * * * * /bin/bash -l -c 'cd OUR_PROJECT_PATH && RAILS_ENV=production bundle exec rake whenever_call --silent'
```

If we see in our production logs, we can see the following.

```bash
# log/production.log
I, [2020-11-15T20:50:02.590009 #53971]  INFO -- : Whenever task
I, [2020-11-15T20:52:02.914487 #54387]  INFO -- : Whenever task
```

As, you can see the logs have 2 minutes of difference.

However, you can see that the task is logging into our `production.log`. If you want to run the cron jobs in development environment, we can use the following.

```bash
whenever --update-crontab --set environment=development
```

Of course, we could specify more when the task runs. Here are some examples:

**Scheduling a task every day at 3:00am**

```ruby
# config/schedule.rb
# ....
every 1.day, at: '3:00 am' do
  rake 'whenever_call'
end
```

```bash
# crontab file
0 3 * * * /bin/bash -l -c 'cd OUR_PROJECT_PATH && RAILS_ENV=production bundle exec rake whenever_call --silent'
```

**Scheduling a task every 2 days at 3:00am and 5:30pm**

```ruby
# config/schedule.rb
# ....
every 2.day, at: ['3:00 am', '5:30 pm'] do
  rake 'whenever_call'
end 
```

```bash
# crontab file
0 3 1,3,5,7,9,11,13,15,17,19,21,23,25,27,29,31 * * /bin/bash -l -c 'cd OUR_PROJECT_PATH && RAILS_ENV=production bundle exec rake whenever_call --silent'

30 17 1,3,5,7,9,11,13,15,17,19,21,23,25,27,29,31 * * /bin/bash -l -c 'cd OUR_PROJECT_PATH && RAILS_ENV=production bundle exec rake whenever_call --silent'
```

**Scheduling a task on monday and sunday at 6:00pm**

```ruby
# config/schedule.rb
# ....
every [:monday, :sunday], at: '6:00 PM' do
  rake 'whenever_call'
end
```

```bash
# crontab file
0 18 * * 1,0 /bin/bash -l -c 'cd OUR_PROJECT_PATH && RAILS_ENV=development bundle exec rake whenever_call --silent'
```

If you want to clear the contents of the crontab file, you can also use the following command.

```bash
# Clear crontab
whenever -c
```

## Final Words

Thanks for reading this post and you can find more info about whenever in its [official github repo](https://github.com/javan/whenever).

If you are also interested in know how to create scheduled jobs in Elixir, I recommend you this amazing post [3 ways to schedule tasks in Elixir I have learned in 3 years working with it](https://blog.kommit.co/3-ways-to-schedule-tasks-in-elixir-i-learned-in-3-years-working-with-it-a6ca94e9e71d).
