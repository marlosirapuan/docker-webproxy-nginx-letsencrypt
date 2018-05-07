# https://gist.github.com/BurntCaramel/2897c0b0e15a777391e20881b066e8f3
FROM ruby:2.5.1-alpine

ENV APP_HOME /src/app
ENV RAILS_ENV production
ENV RAILS_ROOT $APP_HOME
ENV RAILS_SERVE_STATIC_FILES 1
ENV LANG C.UTF-8
ENV PATH /root/.yarn/bin:$PATH

RUN apk update && apk upgrade && \
    apk add --no-cache \
    bash git openssh build-base nodejs \
    tzdata postgresql-dev imagemagick vim apk-cron

RUN apk update \
  && apk add curl bash binutils tar gnupg \
  && rm -rf /var/cache/apk/* \
  && /bin/bash \
  && touch ~/.bashrc \
  && curl -o- -L https://yarnpkg.com/install.sh | bash \
  && apk del curl tar binutils

# Configure the main working directory. This is the base
# directory used in any further RUN, COPY, and ENTRYPOINT
# commands.
RUN mkdir -p $APP_HOME
WORKDIR $APP_HOME

# Copy the Gemfile as well as the Gemfile.lock and install
# the RubyGems. This is a separate step so the dependencies
# will be cached unless changes to one of those two files
# are made.
COPY Gemfile Gemfile.lock ./
RUN gem install bundler && bundle install -j "$(getconf _NPROCESSORS_ONLN)" --retry 5 --without development test

# Copy dependencies for Node.js and instance the packages.
# Again, being separate means this will cache.
COPY package.json yarn.lock ./
RUN yarn install
RUN npm rebuild node-sass --force

# Copy the main application.
COPY . ./

# Precompile Rails assets (plus Webpack)
RUN bundle exec rake assets:precompile

# Will run on port 3000 by default
EXPOSE 3000

# Start puma
CMD bundle exec puma -C config/puma.rb