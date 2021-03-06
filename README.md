Data migrations for Mongoid. [![Build Status](https://travis-ci.org/adacosta/mongoid_rails_migrations.svg?branch=master)](https://travis-ci.org/adacosta/mongoid_rails_migrations)

Gemfile:
```ruby
gem "mongoid_rails_migrations"
```

# How to use

Create migration
```
$ rails generate mongoid:migration <your_migration_name_here>
```

# Using along side ActiveRecord migrations
If your rails app uses both `Mongoid` and `ActiveRecord` migrations then the migration files and rake tasks created by the two gems will cause conflict.
In order to resolve this add the following lines to your `application.rb` file:
```
Mongoid::Migrator.namespace = 'db:mongoid'
Mongoid::Migrator.migrations_path = ['db/mongoid']
```
With the above changes the generated migration files will be created under the `db/mongoid` path instead of the default `db/migrate` path and the rake tasks would change to:

Run migrations:
```
$ rails db:mongoid:migrate
$ rails db:mongoid:migrate:down VERSION=
$ rails db:mongoid:migrate:up VERSION=
$ rails db:mongoid:rollback
$ rails db:mongoid:rollback_to VERSION=
$ rails db:mongoid:migrate:redo
$ rails db:mongoid:migrate:reset
$ rails db:mongoid:migrate:status
$ rails db:mongoid:reseed (handled by mongoid)
$ rails db:mongoid:version
```

To override the default migrations path (`db/migrate`), add the following line to your `application.rb` file:
```
Mongoid::Migrator.migrations_path = ['foo/bar/db/migrate', 'path/to/db/migrate']
```

If you want to use output migration use the hook `after_migrate`
```
Mongoid::Migration.after_migrate = ->(output, name, direction, crash) {
  upload_to_s3(name, output, direction) if crash == false
}
```

# Compatibility

* `1.2.x` targets Mongoid >= `4.0` and Rails >= `4.2`
* `1.1.x` targets Mongoid >= `4.0` and Rails >= `4.2`
* `1.0.0` targers Mongoid >= `3.0` and Rails >= `3.2`
* `0.0.14` targets Mongoid >= `2.0` and Rails >= `3.0` (but < `3.2`)

# Changelog

## Unreleased

## 1.4.0
_08/01/2021_
[Compare master with 1.3.0](https://github.com/adacosta/mongoid_rails_migrations/compare/v1.3.0...master)
* The hook `after_migrate` can be use when migration crash (#56)

## 1.3.0
_17/12/2020_
* Rake Tasks updated to use `migrations_path` instead of hardcoded path (#52)
* Added `after_migrate` hook(#54)

## 1.2.1
_17/01/2019_
* Fix on `db:migrate:status` task to behave like the `ActiveRecord` version (#47)

## 1.2.0
_23/10/2018_
* Added a `rollback_to` task to rollback to a particular version (#17)
* Added a `db:migrate:status` task to list pending migrations (#46)

## 1.1.1
_18/08/2015_
* Added support for Rails5
* First version in Changelog

# Tests

```
$ bundle exec rake
```

# Credits to

* rails
* mongoid
* contributions from the community (git log)

Much of this gem simply modifies existing code from both projects.
With that out of the way, on to the license.

# License (MIT)

Copyright © 2013: Alan Da Costa

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'),
to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

The software is provided 'as is', without warranty of any kind, express or implied, including but not limited to the warranties of
merchantability, fitness for a particular purpose and noninfringement. In no event shall the authors or copyright holders be liable for any
claim, damages or other liability, whether in an action of contract, tort or otherwise, arising from, out of or in connection with the
software or the use or other dealings in the software.
