# Rack basics

## create a config file with the following configurations, in this exampel the root file will be named app.rb
```
# config.ru
require_relative 'app'

run App.new
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
class App
  def call(env)
      ['200', {'Content-Type' => 'text/plain'}, ['Welcome to root the page']]
  end
end
```
## run the server
```
$ bundle exec rackup config.ru -p 9595
```

## basic routing example
env['REQUEST_PATH'] can be used to access the request path, which can be combined with a case statement for basic routing, don't forget to require_relative to reference other ruby pages.

```
require_relative 'name_of_example_file'

class App
  def call(env)
    case env['REQUEST_PATH']
    when '/'
      ['200', {'Content-Type' => 'text/plain'}, ['Welcome to root the page']]
    when '/name_of_example'
      variable = Name_of_example_class.new.method_return_value
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
## run the app at localhost:9595 or curl
```
http://localhost:9090/
curl localhost:9090
```
## reading html files with erb
don't forget to change 'Content-Type' => 'text/html'
erb allows us to write ruby in html files

```
#views/index.erb
<html>
  <body>
    <h2>Hello Pizza!</h2>
    <% random = (0..9).to_a.sample %>
    <%= random %>
  </body>
</html>
```
```
when '/'
  template = File.read("views/index.erb")
  content = ERB.new(template)
  ['200', {'Content-Type' => 'text/html'}, [content.result]]
```
