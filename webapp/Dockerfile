FROM phusion/baseimage:0.9.22
CMD ["/sbin/my_init"]

# UPDATE SYSTEM TO USE SPECIFIC RUBY VERSION
RUN add-apt-repository -y ppa:brightbox/ruby-ng
RUN apt-get update
RUN apt-get install -y libpq-dev git-core curl zlib1g-dev build-essential  \
                       libssl-dev libreadline-dev libyaml-dev libxml2-dev  \
                       libxslt1-dev libcurl4-openssl-dev nodejs wget       \
                       software-properties-common libffi-dev autoconf      \
                       ruby2.4 ruby2.4-dev rubygems ruby-switch tzdata

RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
RUN ruby-switch --set ruby2.4
RUN rm -rf /var/lib/gems/2.4.1/cache/*


# CREATE SERVICE ACCOUNT TO RUN THE WEB SERVER
RUN adduser --disabled-password --gecos "" railsuser
RUN mkdir -p /svr/www
WORKDIR /svr/www


# IMPORT GEMFILE FROM HOST
ADD Gemfile /svr/www/Gemfile
ADD Gemfile.lock /svr/www/Gemfile.lock


# GRANT SERVICE ACCOUNT ACCESS TO THE WEB DIR
RUN chown -R railsuser:railsuser /svr/www
USER railsuser
WORKDIR /svr/www


# INSTALL GEMS
RUN echo "gem: --user-install --env-shebang --no-rdoc --no-ri" > /home/railsuser/.gemrc
ENV PATH /home/railsuser/.gem/ruby/2.4.0/bin:$PATH
ENV GEM_HOME /home/railsuser/.gem/ruby/2.4.0
RUN gem install bundler
RUN gem install rake
RUN bundle install


# IMPORT HOST DIRECTORY
ADD . /svr/www
