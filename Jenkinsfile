pipeline {
 agent { label 'jenkins-d1' }
 environment {
        PATH = "/usr/pgsql-9.6/bin:$PATH"
    }

   stages {
      stage('Build') {
         steps {
            echo env.PATH
            sh  '''#!/bin/bash --login
                   rm -f Gemfile.lock
                   export BUNDLE_GEMFILE=${WORKSPACE}/Gemfile
                   export CHECK='rspec spec'
                   export TORQUEBOX_FALLBACK_LOGFILE=/dev/null
                   export JRUBY_OPTS='--debug'
                   export JDBCUTIL_DBNAME=//127.0.0.1:5432/razor
                   export JDBCUTIL_DBUSER=postgres
                   export JDBCUTIL_DBPASS=
                   export PATH=$PATH
                   export PUPPET_GEM_VERSION='5.3.4'
                   mkdir -p /tmp/repo
                   cp config.yaml.travis config.yaml
                   psql -c 'create database razor;' -U postgres
                   rvm use "jruby-9.1.5.0" --install --binary --fuzzy
                   gem update --system
                   gem install bundler -v 1.10
                   bundle install
                   bundle exec ./bin/razor-admin migrate-database
                   bundle exec rake  $CHECK
              '''
         }
      }
   }
}
