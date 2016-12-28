# ActiveDynamic

Welcome to your new gem! In this directory, you'll find the files you need to be able to package up your Ruby library into a gem. Put your Ruby code in the file `lib/active_dynamic`. To experiment with that code, run `bin/console` for an interactive prompt.

TODO: Delete this and the text above, and describe your gem

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'active_dynamic'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install active_dynamic

## Usage

To make this gem work, first you need to include `HasDynamicAttributes` concern to the model that needs to have dynamic 
attributses. For example, if you have `Profile` model:
 
```ruby
class Profile < ActiveRecord::Base
  include DynamicAttributes::HasDynamicAttributes
  
  # ...
end  
```

After that you need to set a class that will resolve definitions of the dynamic attributes to be created on `Profile` model:

```ruby
# lib/initializers/dynamic_attribute.rb

ActiveDynamic.configure do |config|
  config.provider_class = ProfileAttributeProvider
end

class ProfileAttributeProvider

  # Constructor will receive instance of a model to which dynamic attributes are added
  def initialize(model)
    @model = model    
  end
  
  # This method has to return array of dynamic field definitions.
  # You can get it from the configuration file, DB, etc., depending on your app logic
  def call
    [
      # attribute definition has to specify attribute name, datatype, and optionally default value
      ActiveDynamic::AttributeDefinition.new('age', ActiveDynamic::DataType::Integer, 18),
      ActiveDynamic::AttributeDefinition.new('biography', ActiveDynamic::DataType::Text)
    ]
  end
  
end

```


## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/koss-lebedev/active_dynamic. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

