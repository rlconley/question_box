# Minitest::MatchersVaccine

[![Gem Version](http://img.shields.io/gem/v/minitest-matchers_vaccine.svg)](https://rubygems.org/gems/minitest-matchers_vaccine) [![Build Status](https://secure.travis-ci.org/rmm5t/minitest-matchers_vaccine.svg)](http://travis-ci.org/rmm5t/minitest-matchers_vaccine) [![Code Climate](http://img.shields.io/codeclimate/github/rmm5t/minitest-matchers_vaccine.svg)](https://codeclimate.com/github/rmm5t/minitest-matchers_vaccine)

Adds matcher support to minitest without all the other RSpec-style expectation
_infections_.

Using matchers with RSpec-style expectations requires that we _infect_ the
objects that we are testing with new methods. Matcher implementations are
typically overkill, but there are a lot of good testing libraries that still
insist on standardizing on matchers. These matchers still have some value, and
this gem tries to extract that value with straight-forward assertions that
adhere to the matcher spec.

**Why not use
[minitest-matchers](https://github.com/wojtekmach/minitest-matchers)?** This
gem is actually heavily inspired by and based upon the assertions in
minitest-matchers; however, everything else that minitest-matchers brings to
the table is unnecessary unless you're bent on a true RSpec-style syntax.

## How You Can Help

**If you like this project, please help. [Donate via Gittip][gittip] or [buy me a coffee with Bitcoin][bitcoin].**<br>
[![Gittip](http://img.shields.io/gittip/rmm5t.svg)][gittip]
[![Bitcoin](http://img.shields.io/badge/bitcoin-buy%20me%20a%20coffee-brightgreen.svg)][bitcoin]

**[Bitcoin][bitcoin]**: `1rmm5tv6f997JK5bLcGbRCZyVjZUPkQ2m`<br>
[![Bitcoin Donation][bitcoin-qr-small]][bitcoin-qr-big]

## Need Help?

**You can [book a session with me on Codementor][codementor].**<br>
[![Book a Codementor session](http://img.shields.io/badge/codementor-book%20a%20session-orange.svg)][codementor]

[gittip]: https://www.gittip.com/rmm5t/ "Donate to rmm5t for open source!"
[bitcoin]: https://blockchain.info/address/1rmm5tv6f997JK5bLcGbRCZyVjZUPkQ2m "Buy rmm5t a coffee for open source!"
[bitcoin-scheme]: bitcoin:1rmm5tv6f997JK5bLcGbRCZyVjZUPkQ2m?amount=0.01&label=Coffee%20to%20rmm5t%20for%20Open%20Source "Buy rmm5t a coffee for open source!"
[bitcoin-qr-small]: http://chart.apis.google.com/chart?cht=qr&chs=150x150&chl=bitcoin%3A1rmm5tv6f997JK5bLcGbRCZyVjZUPkQ2m%3Famount%3D0.01%26label%3DCoffee%2520to%2520rmm5t%2520for%2520Open%2520Source
[bitcoin-qr-big]: http://chart.apis.google.com/chart?cht=qr&chs=500x500&chl=bitcoin%3A1rmm5tv6f997JK5bLcGbRCZyVjZUPkQ2m%3Famount%3D0.01%26label%3DCoffee%2520to%2520rmm5t%2520for%2520Open%2520Source
[codementor]: https://www.codementor.io/rmm5t?utm_campaign=profile&utm_source=button-rmm5t&utm_medium=shields "Book a session with rmm5t on Codementor!"

## Installation

Add this line to your application's Gemfile:

    gem "minitest-matchers_vaccine"

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install minitest-matchers_vaccine

## Usage

Includes both `assert_must` and `assert_wont` assertions, but also includes
`must` and `wont` facilitator assertions that automatically default to using
the current `subject` method (aka "let variable") or `@subject` instance
variable.

**NOTE:** This gem does not allow matchers to be used with an expectation
  syntax. Let's avoid infecting the objects we're testing.

### Minitest::Test

```ruby
class UserTest < Minitest::Test
  def setup
    @subject = User.new
  end

  def test_fields_and_associations
    must have_db_column :name
    must belong_to :account
    assert_must have_many(:widgets), @subject
  end

  def test_validations
    must have_valid(:email).when("a@a.com", "foo@bar.com", "dave@abc.io")
    wont have_valid(:email).when(nil, "foo", "foo@bar", "@bar.com")
  end

  # Works with matchers in other libs
  def test_stripping
    assert_must strip_attribute(:name), User.new
  end
end
```

### Minitest::Spec

```ruby
describe User do
  subject { User.new }

  # Works with shoulda-matchers
  it "should have fields and associations" do
    must have_db_column :name
    must belong_to :account
    must have_many :widgets
  end

  # Works with valid_attribute
  it "should validate" do
    must have_valid(:email).when("a@a.com", "foo@bar.com", "dave@abc.io")
    wont have_valid(:email).when(nil, "foo", "foo@bar", "@bar.com")
  end

  # Works with matchers in other libs
  it "should strip attributes" do
    must strip_attribute :name
  end
end
```

## Contributing

1. Fork it ( https://github.com/rmm5t/minitest-matchers_vaccine/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request

## Credits

The idea was originally inspired by the matcher assertions implementation in
[minitest-matchers](https://github.com/wojtekmach/minitest-matchers).

## License

Copyright (c) 2014 Ryan McGeary released under the [MIT
license](http://en.wikipedia.org/wiki/MIT_License)
