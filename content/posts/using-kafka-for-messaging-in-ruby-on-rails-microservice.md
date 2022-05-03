+++
title = "Kafka for messaging in Ruby on Rails Microservice"
date = "2021-11-13"
author = "Daniel Shotonwa"
authorTwitter = "dshow_World"
cover = ""
tags = ["Kafka", "Ruby on Rails", "Messaging", "Redis"]
keywords = ["Kafka", "Ruby on Rails", "Messaging", "Redis"]
description = "I have used some tools for messaging and I feel kafka in Ruby on Rails make it easy and striaghtforward without lot of configuration"
showFullContent = false
+++

> In my company, we decided to make use of microservice and move some of our modules into services. We wanted to do it the right way so we formulated three rules
- Each service should not be dependent on another service. When one service fails, user should still be able to use the platform
- Each service should have its own database, we should not care about duplication.
- Any communication should be via Event and it should be non-blocking.
> I will write an article on how we successfully set the micorservice up later.

We have been using Redis [PubSub](https://redis.io/topics/pubsub) for a while for real time communication and we decided to use it again as a messaging tool to update a service database when a user is created on another database. Sound fair right. It was working as it should but Redis does not persist data, it is an instore database. What if the service is down at the moment and is unable to receive the event, does that mean, our database will not be updated? What if we spin up a new database that needs all the user on our platform, are we going to copy all the data into that new database or is there a way to reply all the events that has been sent. 

That is where [Kafka](https://kafka.apache.org/) comes in, in Kafka website it says and I quotes `Apache Kafka is an open-source distributed event streaming platform used by thousands of companies for high-performance data pipelines, streaming analytics, data integration, and mission-critical applications`. That is exactly what we need, data streaming and persistent data.

There are lot of tools that can be used to integrate kafka with rails. The once I go with with I find easy is:
- [Racecar](https://github.com/zendesk/racecar) : Racecar is a friendly and easy-to-approach Kafka consumer framework.
- [DeliveryBoy](https://github.com/zendesk/delivery_boy): This library provides a dead easy way to start publishing messages to a Kafka cluster from your Ruby or Rails application!

### How to Setup
Install kafka on your local machine. If your are using Mac, using `brew install kafka` is what you need, it will install kafka and zookeeper so you can just start both services using `brew services start zookeeper` and `brew services start kafka`


#### Publishing Events
If your service is the main service and will be publishing to the other service, add DeliveryBoy to your gemfile, as the name implies, it will be the one delivering the event. 😇

```ruby
## Gemfile

gem 'delivery_boy'
```

And then run bundle:

```
$ bundle
```

After this, run its generator to add its config to rails.

```
$ bundle exec rails generate delivery_boy:install
```

We are done, To send event, we can just send event to a particular topic and any consumers listening to that particular topic will get the event.

```ruby
### app/controllers/users_controller.rb
class UsersController < ApplicationController
  def create
    @user = User.create(user_params)

    event = {
      name: "user_created",
      data: {
        id: SecureRandom.uuid,
        user: @user
      }
    }

    DeliveryBoy.deliver_async(event.to_json, topic: "user_activity")
  end
end

```

All consumers will be in charge of discarding the event that they have processed. I added a unique uuid to take care of that. Using the uuid, the consumers can discard the event if that uuid have already being processed. It can be stored in the database or anywhere based on preference.

#### Consuming Events
For consuming event, we will be using racecar. It is straighforward to implement.


```ruby
## Gemfile

gem 'racecar'
```

And then run bundle:

```
$ bundle
```

After this, run its generator to add its config to rails.

```
$  bundle exec rails generate racecar:install
```

We just need to create a consumer that will subscribe to our topic and perform an action.

```ruby
### app/consumers/user_created_consumer.rb
class UserCreatedConsumer < Racecar::Consumer
  subscribes_to "user_activity"

  def process(message)
    data = JSON.parse(message.value)
    ## check if data.id is in the database or wherever and discard the message
    User.create(data['user'])
  end
end
```

That's all, you can now publish and subscribe to an event using Kafka and rails.
If you have a question, you can reach me on github. [danielshow](https://github.com/Danielshow)

[Buy me a Coffee](https://www.buymeacoffee.com/danielshow)
