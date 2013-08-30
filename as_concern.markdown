## About ActiveSupport::Concern

### Task1 

#### mixed in instance method and class method

```ruby

module M
  def self.included(base)
    base.extend ClassMethods
    base.class_eval do
      scope :disabled, -> { where(disabled: true) }
    end
  end

  module ClassMethods
    ...
  end
end

```


### include vs extend vs included


