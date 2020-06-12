+++
title = "Using Rails 6 ActionText"
date = "2019-08-10"
author = "Daniel Shotonwa"
authorTwitter = "Dshow_World" #do not include @
cover = ""
tags = ["Rails6", "ActionText", "Programming"]
keywords = ["Rails6", "ActionText", "Programming"]
description = "I have started using Rails 6 even though it is not fully out, the integration of webpack, Action Cable, ActionText, ActionView and lot of other background task have improved Rails and make it faster. The one I am particularly happy about is Action Text."
showFullContent = false
+++

I have started using Rails 6 even though it is not fully out, the integration of webpack, Action Cable, ActionText, ActionView and lot of other background task have improved Rails and make it faster. The one I am particularly happy about is Action Text.

Design an Application for Blogs have never been made easy before we'd integrate other external libraries and configure the application. Now, with the integration of Action Text which uses Trix which was design by Basecamp developers, just some lines of code, we should be able to design a fully functional blog.

In this article, we will be building an application where users can create a blog using Action text as their textarea in Rails 6.

### Prerequisites

Before we begin, make sure you have ruby version ≥ 2.2 and rails version ≥ 6 installed on your local machine.

If your ruby version is not up-to-date, you can update with a ruby version manager such as rvm or rbenv

Rails 6 is not fully out but we can install the pre version rails-6.0.0.rc1.

```bash
$ rvm install "ruby-2.4.6"
or
$ rbenv install 2.4.6## You can install rails by running
$ gem install rails --pre
```
That’s all you need, we are good to start.

Project Setup
```bash
$ rails new action-text-tutorial --database=postgresql
$ cd action-text-tutorial
```

With this, all the files and folders needed will be installed. You should see Yarn is also installed and webpack to compile our javascript file.

Since we will be using PostgreSQL, lets added our database credentials. open config/database.yml and fill this with your data.
```ruby
development:
  <<: *default
  database: action-text-tutorial_development
  username: YOUR_POSTGRES_USERNAME
  password: YOUR_POSTGRES_PASSWORDtest:
test:
  <<: *default
  database: action-text-tutorial_development
  username: YOUR_POSTGRES_USERNAME
  password: YOUR_POSTGRES_PASSWORD
```
Create the database by running: 

```bash
$ rails db:create
```
Finally, run the server by running:

```bash
$ rails server
```
That's all we need for the setup lets create the migrations and models needed for this application. we will just build a basic application so we need a blogs model that contains title and body field. You can add a user model if you want to move it forward.

### Models
Create the models needed using

```
$ rails generate model Blog title:string body:text
```
After that run your migration using
```
$ rails db:migrate
```

**Action Text Install**
Run this line to install actiontext dependencies
```bash
$ rails action_text:install
```
This will install the various files needed and also add Trix styling to the application. You can checkout trix.scss file to update the styles.

Run migration to install the migrations added by action-text
```bash
$ rails db:migrate
```
Add this line to your model/blogs.rb to ensure body field uses action text

```ruby
 class Blog < ApplicationRecord
   has_rich_text :body
 end
```
That's all for the models, let's create a blog resource
```bash
$ rails g controller Blogs index new create show
```
Add this to blogs_controller
```ruby
class BlogsController < ApplicationController
  def index
    @blogs = Blog.all
  end

  def show
    @blog = Blog.find(params[:id])
  end

  def new
    @blog = Blog.new
  end

  def create
    @blog = Blog.new(blogs_params)

    if @blog.save
      redirect_to blogs_path
    else
      render 'new'
    end
  end
  
  private

  def blogs_params
    params.require(:blog).permit(:title, :body)
  end
end
```
Add this to blogs/new.html.erb to create the form. Instead of using textarea for the body field, we used rich_text_area, a helper from action-text.
```html
<h1>Blog#new</h1>
<p>Find me in app/views/blog/new.html.erb</p>

<%= form_for @blog do |f| %>
  <%= f.label :title %>
  <%= f.text_field :title %>

  <%= f.label :body %>
  <%= f.rich_text_area :body %>

  <%= f.submit %>
<% end %>
```
Run your server and head over to http://localhost:3000/blogs/new , You should see the action text view in its beauty. Write a little blog and submit, you will be redirected to the index page.

Let's add this to the index page to display all blogs
```html
<h1>Blog#index</h1><p>Find me in app/views/blog/index.html.erb</p>
<h1>All blogs</h1>

<% @blogs.each do |blog| %>  
<li><%= link_to blog.title, blog_path(blog) %></li>

<% end %>

Add this to blogs show page to display one blog. 

blogs/show.html.erb

<h1>Show page</h1>

<h1> <%= @blog.title %></h1>
<p><%= @blog.body %></p>
```
That's all, you can add edit action, delete and others.

If you need the boilerplate that contains all this, head over to this repository and clone it. https://github.com/Danielshow/action-text-tutorial

I am Danielshow. 🕺🏽👻