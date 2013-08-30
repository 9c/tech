## About ActiveSupport::Concern

### Task1 

```ruby
class Bbcom < ActiveRecord::Base
  scope :golden, ->(flag) { where(golden: flag)}
  has_many :page_contents
  
  def contents
  end
  
  def self.draft_version
  end
  
end
```

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


