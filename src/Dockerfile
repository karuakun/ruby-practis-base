FROM ruby:2.4.3-alpine as base

WORKDIR /myapp
ADD Gemfile* ./

RUN apk update \
    && apk add --no-cache \
        nodejs \
        tzdata \
        mariadb-client-libs \
        build-base \
        linux-headers \
        mysql-dev \
    && cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime \
    && echo "Asia/Tokyo" > /etc/timezone \
    && gem install bundler \ 
    && bundle install --jobs=4

ADD . .
