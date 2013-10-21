HStore Switches
------------

Make boolean properties in a Postgresql hstore column act like your average, everyday ActiveRecord boolean field. 

Requirements
------------

* Rails 4.0 or greater
* Postgresql 8.4+ with contrib package
* Read [activerecord-postgres-hstore](https://raw.github.com/engageis/activerecord-postgres-hstore) for index creation

Installation
------------

1. Add `gem hstore_switches` to your Gemfile
2. `bundle install`
3. Tag fields in your model code as `hstore_switches` as shown in Usage below.

Usage
-----

```ruby
# defining flags
class User < ActiveRecord::Base
  hstore_switches :active, :admin
  hstore_switches :customer, :vendor, :drop_ship, field: "user_type"
end

class Group < ActiveRecord::Base
  hstore_switches :active, :public, :invite_only, scopes: false
end

# setting flags
u = User.new
u.active = true
u.vendor = false

# automatic scope creation
User.active.to_sql        #=> SELECT * FROM users WHERE (defined(switches, 'active') IS TRUE)
User.not_drop_ship.to_sql #=> SELECT * FROM users WHERE (defined(user_type, 'drop_ship') IS NOT TRUE)
```

* `field` option defaults to `switches`
* `scopes: false` disables scope creation.
