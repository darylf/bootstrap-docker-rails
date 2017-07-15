## Overview

This project is based on [Phusion's Baseimage-docker](http://phusion.github.io/baseimage-docker/) 
image, and the [Brightbox-ng package](https://www.brightbox.com/docs/ruby/ubuntu/).

It installs Ruby 2.4 and Rails 5.1.2 on the docker container, and sets up a "webuser" user to 
run the application (instead of root)

## Dependencies

* Docker
* Docker Compose

## How To Create a New Development App

### Build the project

    docker-compose build
    docker-compose run web rails new . --force --database=postgresql

### Modify the `config/database.yml` as follows:

    default: &default
      adapter: postgresql
      encoding: unicode
      host: db
      username: postgres
      password:
      pool: 5
    
    development:
      <<: *default
      database: myapp_development
    
    test:
      <<: *default
      database: myapp_test

### Rebuild the Docker Image

Since `rails new` has updated the Gemfile, we need to rebuild the docker image:

    docker-compose build

### Start the Application

    docker-compose up

### Useful Commands

    # Create database
    docker-compose run webapp rake db:create
    
    # Migrate database
    docker-compose run webapp rake db:migrate

    # Stop the application
    docker-compose down
