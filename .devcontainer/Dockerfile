FROM debian:latest

RUN apt-get update && apt-get full-upgrade -y

# apt packages
RUN apt-get install -y ruby-full build-essential zlib1g-dev git

# ruby packages
RUN gem install jekyll bundler

# only required for dev environment
# dependencies to shrink photos using the photo-shrink.py script
RUN apt install -y imagemagick python3
