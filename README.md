[![Build Status](https://secure.travis-ci.org/NickolasVashchenko/stopgap_13632.svg)](http://travis-ci.org/NickolasVashchenko/stopgap_13632)
[![Build Status](https://ci.appveyor.com/api/projects/status/cqgu4tce6of44c9x?svg=true)](https://ci.appveyor.com/api/projects/status/cqgu4tce6of44c9x?svg=true)

## Description
A gem with a temporary stopgap workaround for MRI Ruby versions 2.2.7, 2.3.4, 2.4.1 for the bug https://bugs.ruby-lang.org/issues/13632 until the proper fix gets backported.
No other version besides mentioned 3 are affected.
The mentioned versions are currently most recent ones.

## How does this bug look?
You're getting similar exception erratically and in places where it should not happen:
```ruby
Traceback (most recent call last):
    1: from test2.rb:9:in `block (2 levels) in <main>'
test2.rb:9:in `sysopen': stream closed in another thread (IOError)
```

## Installation
```ruby
if %w(2.2.7 2.3.4 2.4.1).include? RUBY_VERSION
  gem "stopgap_13632", "~> 1.0", :platforms => ["mri", "mingw", "x64_mingw"]
end

```

## Usage
```
require 'stopgap_13632'
```
And when an "IOError: stream closed" happens in a thread, accessing a busy IO:
```
rescue IOError
  Thread.current.purge_interrupt_queue
end
```
It will unblock the thread and allow it to proceed.

## Contributing
```bash
rake compile
rake test
```
