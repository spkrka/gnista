# Gnista

Gnista is a wrapper for the database/hashstore [Sparkey](http://github.com/spotify/sparkey) written for Ruby. Gnista packs all the native features of Sparkey into a shiny red package, easy to use.

## Setup

You will need Sparkey, obviously. It's really easy to install. Head over to [http://github.com/spotify/sparkey](http://github.com/spotify/sparkey) and install it, I'll wait..

Now, add this line to your application's Gemfile:

    gem 'gnista'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install gnista

## Usage

You can read about Sparkey [here](http://github.com/spotify/sparkey).

```ruby
require 'gnista'

logwriter = Gnista::Logwriter.new "mylog.log" # no compression
logwriter = Gnista::Logwriter.new "mylog.log", 4000 # 4k compression block size
logwriter = Gnista::Logwriter.new "mylog.log", :append # append to existing log

logwriter.put "key", "value" # put entry
logwriter.del "key" # delete entry

logwriter.flush

logreader = Gnista::Logreader.new "mylog.log"
logreader.each {|key,value| puts key, value }

Gnista::Hash.write "mylog.hash", "mylog.log" # no preferred hash size
Gnista::Hash.write "mylog.hash", "mylog.log", 4 # 32 bit murmurhash3_x86_32
Gnista::Hash.write "mylog.hash", "mylog.log", 8 # lower 64-bit part of murmurhash3_x64_128

hash = Gnista::Hash.new "mylog.hash", "mylog.log"
hash.each {|key,value| puts key, value }
hash.get "key" # => "value" or nil

hash.maxkeylen # largest key length
hash.maxkeylen # largest value length
hash.length
hash.collisions

# Don't forget to close!
logwriter.close
logreader.close
hash.close
```


## Contributing

Use the following commands to build the native extensions and test:

	$ rake make
	$ rake test

Then:

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
