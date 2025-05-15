# version 0.0.1
# adapted from https://help.github.com/en/articles/setting-up-your-github-pages-site-locally-with-jekyll
FROM ruby:slim-buster

RUN apt-get update \
    && apt-get -y install --no-install-recommends apt-utils dialog 2>&1 \
    #
    # Verify git, build-tools and locales installed
    && apt-get -y install git build-essential locales \
    #
    # Clean up
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

# Install Bundler
RUN gem install bundler
# Install Jekyll and other dependencies from the GitHub Pages gem
ADD Gemfile .
RUN bundle install

# Fix local problem
# see https://github.com/jekyll/jekyll/issues/4268#issuecomment-167406574
RUN dpkg-reconfigure locales \
    && locale-gen C.UTF-8 \
    && /usr/sbin/update-locale LANG=C.UTF-8 \

    && echo 'en_US.UTF-8 UTF-8' >> /etc/locale.gen \
    && locale-gen

# Set default locale for the environment
ENV LC_ALL C.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8

CMD [ "bundle", "exec", "jekyll", "serve", "--livereload", "--host", "0.0.0.0" ]
