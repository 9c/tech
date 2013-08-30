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

#### solution 1

```ruby
module Editorial
  module Page1
    def contents
    end
  end
  
  module Page2
    def draft_version
    end
  end
end

class Bbcom < ActiveRecord::Base
  include Editorial::Page1
  extend Editorial::Page2
end

```


#### mixed in instance method and class method

```ruby

module Editorial
  module page
    def self.included(base)
      base.extend ClassMethods
      base.class_eval do
        scope :golden, ->(flag) { where(golden: flag) }
      end
    end
    
    def contents
    end

    module ClassMethods
      def draft_version
      end
    end
  end
end

```


### include vs extend vs included


