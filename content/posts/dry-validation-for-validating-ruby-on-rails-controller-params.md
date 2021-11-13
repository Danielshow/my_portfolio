+++
title = "Using Dry Validation for validating Ruby on Rails Controller params"
date = "2021-11-13"
author = "Daniel Shotonwa"
authorTwitter = "dshow_World"
cover = ""
tags = ["Validation", "Ruby on Rails", "Dry-validation"]
keywords = ["Validation", "Ruby on Rails", "Dry-validation"]
description = "A better way to validate params without code duplication"
showFullContent = false
+++

>  I have being using Joi Validation for almost all my node project, getting to Rails, I am not always happy with how I validate writing different functions to validate request and sometimnes, it makes my controller bloated. I am not always happy about this, little did I know there is a gem for that. 

In all my Node APIs, I usually create schema, add the schema to the middleware, to validate incoming requests and throws error if some rules are not followed. The good thing about Joi is that it has lot of functions to vakidate if something is a number, string etc.

I have been looking for a rails version of JOI but I have not been able to get, until today I was reading an article on Microservice in rails and a gem was mentioned which I check out and viola!! it has all what I need.

[dry-validation](https://github.com/dry-rb/dry-validation) is a data validation library that provides a powerful DSL for defining schemas and validation rules. It is very easy to use, define your contracts and call it in your controller. Sound Easy right.

![](https://user-images.githubusercontent.com/24846513/141614969-e63a8ddd-968b-416c-883f-b6c39f936c1c.png)

An example of what I have been doing before I start using the gem, what if I have lot of parameter to validate, is the parameter a date, is start_date greater than end_date and the likes, the file will become bloated. Some can say I can create a module for that, but it still look dirty to me.

I moved my validation to a library that makes everything easy.

### How to Setup
```ruby
## Gemfile

gem 'dry-validation'
```

And then run bundle:

```
$ bundle
```

After this, you are good to go.

The next thing is to create a contracts.

```ruby
# app/contracts/profile_contract.rb
class ProfileContract < Dry::Validation::Contract
  params do
    required(:email).filled(:string)
    required(:username).filled(:string)
    required(:accent).filled(:string)
  end

  rule(:email) do
    unless /\A[\w+\-.]+@[a-z\d\-]+(\.[a-z\d\-]+)*\.[a-z]+\z/i.match?(value)
      key.failure('has invalid format')
    end
  end

  values = ['blue', 'yellow']
  rule(:accent) do
    key.failure('must be one of Blue or Yellow') if values.include value
  end
end
```

In our controller, we can easily call the contract and do our validation.

```ruby
class profileController < ApplicationController
  def create
    contract = ProfileContract.new
    contract.call(profile_params)

    render :json { errors: contract.errors }  if contract.errors
    ## Save 
  end
end
```

Thats all, it makes the controller lean and has a railsey way of doing things. I love Rails. 
If you have a question, you can reach me on github. [danielshow](https://github.com/Danielshow)