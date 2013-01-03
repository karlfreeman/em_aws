# EmAws
An EM-Synchrony handler for Ruby [AWS-SDK](https://github.com/amazonwebservices/aws-sdk-for-ruby/)

## Installation

em_aws is available through [Rubygems](https://rubygems.org/gems/em_aws) and can be installed via:

    $ gem install em_aws

## Rails 3 setup (no rails 2 sorry)
Setup [AWS-SKD](https://github.com/amazonwebservices/aws-sdk-for-ruby/blob/master/README.rdoc) as you would normally.

Assuming you've already setup async-rails, add em_aws to you gemfile:
    
    gem 'em_aws'

Then run:
    
    bundle install

In your environments files add:

    require 'aws-sdk'
    require 'aws/core/http/em_http_handler'
    AWS.eager_autoload! # AWS lazyloading is not threadsafe
    AWS.config(
      :http_handler => AWS::Http::EMHttpHandler.new(
      :proxy => {:host => "http://myproxy.com", :port => 80}
       ));

Your done. 

All requests to AWS will use EM-Synchrony's implementation of em-http-request for non-block HTTP request and fiber management.

## Connection Pooling (keep-alive)
To enable connection pooling set the :pool_size to anything greater than 0. By default :inactivity_timeout is set
to 0 which will leave the connection open for as long as the client allows. Connects
are created lazy, so pools grow until they meet the set pool size.
    
    require 'aws-sdk'
    require 'aws/core/http/em_http_handler'
    AWS.config(
      :http_handler => AWS::Http::EMHttpHandler.new({
        :pool_size => 20,
        :inactivity_timeout => 30, # number of seconds to timeout stale connections in the pool
        :never_block => true, # if we run out of connections create a new one but 
                            # close and do return it to the pool when done
        :proxy => {:host => "http://myproxy.com",:port => 80})
    )

## References

  [aws-sdk](https://github.com/amazonwebservices/aws-sdk-for-ruby)

  [Async-Rails](https://github.com/igrigorik/async-rails)

  [Phat](http://www.mikeperham.com/2010/04/03/introducing-phat-an-asynchronous-rails-app/)

## Contributing to em_aws
 
* Check out the latest master to make sure the feature hasn't been implemented or the bug hasn't been fixed yet
* Check out the issue tracker to make sure someone already hasn't requested it and/or contributed it
* Fork the project
* Start a feature/bugfix branch
* Commit and push until you are happy with your contribution
* Make sure to add tests for it. This is important so I don't break it in a future version unintentionally.
* Please try not to mess with the Rakefile, version, or history. If you want to have your own version, or is otherwise necessary, that is fine, but please isolate to its own commit so I can cherry-pick around it.

## Thanks

Code based on HTTParty Hander in [aws-sdk](https://github.com/amazonwebservices/aws-sdk-for-ruby/blob/master/README.rdoc)

## License

EmAws [license](https://github.com/JoshMcKin/em_aws/blob/master/LICENSE.txt)
AWS-SDK [license](https://github.com/amazonwebservices/aws-sdk-for-ruby/blob/master/LICENSE.txt)
