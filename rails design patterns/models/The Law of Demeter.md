# The Law of Demter #

> An object can call methods on a related object but that it should not reach through that object to call a method on a third related object. "use only one dot."

```ruby
class Address < ActiveRecord::Base
  belongs_to :customer
end

class Customer < ActiveRecord::Base
  has_one :address
  has_many :invoices
end

class Invoice < ActiveRecord::Base
  belongs_to :customer
end
```

Things become ugly when `@invoice.customer.address.street`.  The problem arises when `Address` class is changed. To solve this, use the law of demeter.

```ruby
class Address < ActiveRecord::Base
	belongs_to :customer
end

class Customer < ActiveRecord::Base
	has_one :address
	has_many :invoices

	def street
		address.street
	end

	def city
		address.city
	end

	def state
		address.state
	end

	def zip_code
		address.zip_code
	end
end

class Invoice < ActiveRecord::Base
	belongs_to :customer

	def customer_name
		customer.name
	end

	def customer_street
		customer.street
	end

	def customer_city
		customer.city
	end

	def customer_state
		customer.state
	end

	def customer_zip_code
		customer.zip_code
	end
end
```

This is so cluttered, but it can be solved by using rails `delegate`:

```ruby
class Address < ActiveRecord::Base
	belongs_to :customer
end

class Customer < ActiveRecord::Base
	has_one :address
	has_many :invoices
	
	delegate :street, :city, :state, :zip_code, :to => :address
end

class Invoice < ActiveRecord::Base
	belongs_to :customer

	delegate :name,
						:street,
						:city,
						:state,
						:zip_code,
						:to => :customer,
						:prefix => true
end
```








