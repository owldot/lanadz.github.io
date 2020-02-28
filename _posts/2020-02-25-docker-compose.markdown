---
layout: post
title: 'Docker-compose up'
date: 2020-02-25
tags:
  - terminal
keywords:
  - bundle docker
---

I have recently started to use `docker` for [OwlDot](owldot.com) and faced an issue that changes are not automaticaly picked up from `Gemfile` and `bundle install` isn't run.
To make it happen I needed to run `docker-compose up --build` , not just `docker-compose up` as usual. I need to build it 😅. Now it looks so obvious 😅

<!--more-->

my `Dockerfile`

```docker
FROM ruby:2.7.0

RUN apt-get update -qq && apt-get install -y nodejs postgresql-client

RUN mkdir /app
WORKDIR /app

COPY Gemfile /app/Gemfile
COPY Gemfile.lock /app/Gemfile.lock
# Uses bundler v2 now: https://bundler.io/guides/bundler_2_upgrade.html
RUN bundle install

COPY . /app

# Add a script to be executed every time the container starts.
COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
EXPOSE 3001

# Start the main process.
CMD ["rails", "server", "-b", "0.0.0.0"]
```

and `docker-compose.yml`

```yaml
version: '3'

services:
  db:
    image: postgres
    ports:
      - '5431:5432'
    volumes:
      - db:/var/lib/postgresql/data
    logging:
      driver: none
  redis:
    image: redis
    volumes:
      - redis:/data
  web:
    build: .
    command: bash -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3001 -b '0.0.0.0'"
    volumes:
      - .:/app
    ports:
      - 3001:3001
    depends_on:
      - db
      - redis
    env_file:
      - '.env'

volumes:
  db:
  redis:
```

I feel like I need to learn and share more on docker because I didn't use
it on any of projects I worked on and basically this my first time playing with it
