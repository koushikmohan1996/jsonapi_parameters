# JsonApi::Parameters
Simple JSON:API compliant parameters translator.

#### The problem

JSON:API standard specifies not only responses (that can be handled nicely, using gems like [fast_jsonapi from Netflix](https://github.com/Netflix/fast_jsonapi)), but also the request structure. 

#### The solution

As we couldn't find any gem that would make it easier in Rails to use these structures, we decided to create something that will work for us - a translator that transforms JSON:API compliant request parameter strucure into a Railsy structure.

## Usage

### Installation
Add this line to your application's Gemfile:

```ruby
gem 'jsonapi_parameters'
```

And then execute:

```bash
$ bundle
```

Or install it yourself as:

```bash
$ gem install jsonapi_parameters
```

### Rails

Usually your strong parameters in controller are invoked this way:

```ruby
def create
  model = Model.new(create_params)
  
  if model.save
    ...
  else
    head 500
  end
end

private

def create_params
  params.require(:model).permit(:name)
end
```

With jsonapi_parameters, the difference is just the params:

```ruby
def create_params
  params.from_jsonapi.require(:model).permit(:name)
end
```

#### Casing

If the input is in a different convention than `:snake_case`, you should specify that while calling `.from_jsonapi`, for instance: `.from_jsonapi(:camel)`. **The value does not really matter, as anything different than `:snake` will result in deep keys transformation provided by [ActiveSupport](https://apidock.com/rails/v4.1.8/Hash/deep_transform_keys).**

### Plain Ruby / outside Rails

```ruby

params = { # JSON:API compliant parameters here
	# ...
}

class Translator
  include JsonApi::Parameters
end
translator = Translator.new

translator.jsonapify(params)
```

#### Casing

If the input is in a different convention than `:snake_case`, you should specify that while calling `.jsonapify`, for instance: `.jsonapify(params, naming_convention: :camel)`. **The value does not really matter, as anything different than `:snake` will result in deep keys transformation provided by [ActiveSupport](https://apidock.com/rails/v4.1.8/Hash/deep_transform_keys).**

## License
The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
