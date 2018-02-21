# Dirwatch

 Author::      Gavin Kistner (mailto:!@phrogz.net)
 Copyright::   Copyright (c)2004 Gavin Kistner
 License::     See http://Phrogz.net/JS/_ReuseLicense.txt for details
 Version::     0.9.6, 2004-Oct-11
 Full Code::   link:../DirectoryWatcher.rb

 The DirectoryWatcher class keeps an eye on all files in a directory, and calls
 methods of your making when files are added to, modified in, and/or removed from
 that directory.

 The +on_add+, +on_modify+ and +on_remove+ callbacks should be Proc instances
 that should be called when a file is added to, modified in, or removed from 
 the watched directory.

 The +on_add+ and +on_modify+ Procs will be passed a File instance pointing to
 the file added/changed, as well as a Hash instance. The hash contains some
 saved statistics about the file (modification date (<tt>:date</tt>),
 file size (<tt>:size</tt>), and original path (<tt>:path</tt>)) and may also
 be used to store additional information about the file.

 The +on_remove+ Proc will only be passed the hash that was passed to +on_add+
 and +on_modify+ (since the file no longer exists in a way that the watcher
 knows about it). By storing your own information in the hash, you may do
 something intelligent when the file disappears.

 The first time the directory is scanned, the +on_add+ callback will be invoked
 for each file in the directory (since it's the first time they've been seen).

 Use the +onmodify_checks+ and +onmodify_requiresall+ properties to control
 what is required for the +on_modify+ callback to occur.

 If you do not set a Proc for any one of these callbacks; they'll simply
 be ignored.

 Use the +autoscan_delay+ property to set how frequently the directory is
 automatically checked, or use the #scan_now method to force it to look
 for changes.

 <b>You must call the #start_watching method after you create a DirectoryWatcher
 instance</b> (and after you have set the necessary callbacks) <b>to start the
 automatic scanning.</b>

 The DirectoryWatcher does not process sub-directories of the watched
 directory. Any item in the directory which returns +false+ for <tt>File.file?</tt>
 is ignored.

TODO: Delete this and the text above, and describe your gem

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'dirwatch'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install dirwatch

## Usage

 Example:

    device_manager = Dirwatch::DirectoryWatcher.new( '/Users/lello107', 2 )
    device_manager.name_regexp = /^[^.].*\.rb$/
    
    device_manager.on_add = Proc.new{ |the_file, stats_hash|
       puts "Hey, just found #{the_file.inspect}!" 
    }
    
    device_manager.on_modify = Proc.new{ |the_file, stats_hash|
       puts "Hey, #{the_file.inspect} just changed."
    }
    
    device_manager.on_remove = Proc.new{ |stats_hash|
       puts "Whoa, the following file just disappeared:"
       stats_hash.each_pair{ |k,v|
          puts "#{k} : #{v}"
       }
    }
    
    device_manager.start_watching

## Development

After checking out the repo, run `bin/setup` to install dependencies. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/[USERNAME]/dirwatch.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

