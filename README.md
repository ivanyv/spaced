# Spaced

Spaced is a super simple and convenient way to isolate and namespace a collection of related methods.

```ruby
class User
  include Spaced

  # Pass a black with a bunch of methods.
  namespace :twitter do
    def create(msg)
      api.create_tweet msg
    end

    def read(id)
      api.read_tweet id
    end

    def call(msg)
      create msg
    end

    def predicate
      subject.twitter_id?
    end

    private

      def api
        @api ||= TwitterClient.new(api_token: parent.api_token)
      end
  end

  # Or pass a predefined class, which should subclass `Spaced::Base`.
  namespace :facebook, Facebook::Api
end

user = User.new
id = user.twitter.create("Spaced man!")
user.twitter.read(id)
user.twitter!("Spaced man!") # calls the `call` method.
user.twitter? # calls the `predicate` method.
```

In the example above, `namespace` creates and initializes a new class `Twitter` and returns it from the `#twitter` method. The parent class - in this case `User` - is available at `#parent` and `@parent` from within the namespace.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'spaced'
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake test` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and the created tag, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/joelmoss/spaced. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [code of conduct](https://github.com/joelmoss/spaced/blob/master/CODE_OF_CONDUCT.md).

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

## Code of Conduct

Everyone interacting in the Spaced project's codebases, issue trackers, chat rooms and mailing lists is expected to follow the [code of conduct](https://github.com/joelmoss/spaced/blob/master/CODE_OF_CONDUCT.md).
