# Rack basics

## create a config file with the following configurations
```
# config.ru
require_relative 'name_of_root_rb_file'

run name_of_root_rb_file.new
```

## create a Gemfile to access rack gem
```
# Gemfile

source "https://rubygems.org"

gem 'rack', '~> 2.0.1'
```
```
$bundle install
```

## create a root ruby file to serve pages
```
class name_of_root_rb_file
  def call(env)
      ['200', {'Content-Type' => 'text/plain'}, ['Welcome to root the page']]
  end
```
## run the server
```
$ bundle exec rackup config.ru -p 9595
```

## basic routing example
env['REQUEST_PATH'] can be used to access the request path, which can be combined with a case statement for basic routing, don't forget to require_relative to reference other ruby pages.

```
require_relative 'name_of_example'

class name_of_root_rb_file
  def call(env)
    case env['REQUEST_PATH']
    when '/'
      ['200', {'Content-Type' => 'text/plain'}, ['Welcome to root the page']]
    when '/name_of_example'
      variable = Name_of_example.new.method_return_value
      ['200', {'Content-Type' => 'text/plain'}, [variable]]
    else
      [
        '404',
        {'Content-Type' => 'text/plain', 'Content-Length' => '13'},
        ['404 Not Found']
      ]
    end
  end
end
```
