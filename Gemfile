# If you have OpenSSL installed, we recommend updating
# the following line to use "https"
source 'http://rubygems.org'

gem "hitimes", "~> 1.2.2"

gem "rouge", "1.7.2"

gem "middleman", "~>3.3.0"

# For syntax highlighting
gem "middleman-syntax"

# Plugin for middleman to generate Github pages
gem 'middleman-gh-pages'

# Live-reloading plugin
gem "middleman-livereload", "~> 3.3.0"

gem 'redcarpet', '~> 3.1.1'

# For faster file watcher updates on Windows:
gem "wdm", "~> 0.1.0", :platforms => [:mswin, :mingw]

# Cross-templating language block fix for Ruby 1.8
platforms :mri_18 do
  gem "ruby18_source_location"
end

group :development do
  gem 'rb-inotify', :require => false
  gem 'rb-fsevent', :require => false
  gem 'rb-fchange', :require => false
end

gem "rake", "~> 10.3.0"

gem 'therubyracer', :platforms => :ruby
