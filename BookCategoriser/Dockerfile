# syntax = docker/dockerfile:1

# Make sure RUBY_VERSION matches the Ruby version in .ruby-version and Gemfile
ARG RUBY_VERSION=3.3.4
FROM registry.docker.com/library/ruby:$RUBY_VERSION-slim as base

# Install packages needed to build gems
RUN apt-get update -yqq
RUN apt-get install -yqq --no-install-recommends nodejs
RUN apt-get install -y build-essential libgmp-dev

COPY . /user/src/app/
ARG SECRET_KEY_BASE 

WORKDIR /app

# Copy the Gemfile and Gemfile.lock to the container
COPY BookCategoriser/Gemfile BookCategoriser/Gemfile.lock ./

# Set production environment
ENV RAILS_ENV=production 
ENV SECRET_KEY_BASE=$(SECRET_KEY_BASE)

RUN gem install bundler:2.5.17
RUN bundle install
RUN bundle exec rake db:create db:migrate --trace

RUN rails assets:precompile

CMD ["rails", "server"]
