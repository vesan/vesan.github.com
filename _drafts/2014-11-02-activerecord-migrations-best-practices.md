---
layout: post
title: ActiveRecord migration best practices
tags: [ruby, rails, programming, activerecord, database]
---

There are certain guidelines I follow when writing migrations using the ActiveRecord migrations library. The guidelines have formed during the years I've been working with Ruby on Rails.

Here is an example migration class that follows all the guidelines reviewed in this post:

```ruby
class AddAreaToPlots < ActiveRecord::Migration
  class Plot < ActiveRecord::Base; end

  def up
    add_column :plots, :area, :float

    Plot.find_each do |plot|
      plot.area = plot.width * plot.height
      plot.save!
    end
  end

  def down
    remove_column :plots, :area
  end
end
```

## Redefine ActiveRecord classes

If possible, ActiveRecord models should not be used in migrations. Migrations should be simple and failproof transformations of database schema and data. Using complex model objects that complicate the migration should be avoided.

If you have strong SQL skills, you can use it for data transformations. Well written SQL is often faster than using ActiveRecord. For schema changes I would use the ActiveRecord methods which are readable and give extra features provided by the framework (most notably the migration/rollback support talked later in this post).

If you want to use the familiar ActiveRecord API to do the data transformations, the ActiveRecord classes should be redefined. As shown in the example, the `Plot` class has been redefined inside the migration class. The redefinition overrides the model class from the application code and makes it a bare ActiveRecord class. This means it has no validations, associations or callbacks. If some of the callbacks should be called in the migration, those can be reimplemented in the redefined class. Same goes for methods in the model class.

Why go through this trouble? When the application changes, the models change with it. It's possible that the models change in a way that break the migrations. Or the models can be renamed or removed which will also break the migrations.

It might feel weird but it just works, makes your migrations more robust, and protects you from trouble in the future.

## Use find_each

When modifying records, `Model.find_each` should be used to fetch the records in batches. If `Model.all` is used on a big database table, ActiveRecord will instantiate objects for all rows in the table which will consume all the available memory and the process starts swapping memory like crazy slowing the migration.

By default `find_each` loads the records in batches of 1000 but that can be configured. Only drawback of `find_each` is that you can't use `order`, `offset` or `limit`. This is because `find_each` orders the records by id and then uses offset and limit to control the loading of the batches. Other than these caveats, there are no drawbacks on using `find_each`. Filtering the records using `where` works just fine.

## Don't change the data using the change method

The `change` method for migration classes was introduced in Rails 3.1 to make the writing of migrations simpler. Using `change` and methods that support inversing themselves, you can write migrations like this:

class AddAreaToPlots < ActiveRecord::Migration
  def change
    add_column :plots, :area, :float
  end
end

When the migration are run by ActiveRecord, it executes the "up" version of the command adding the area column. On rollback it runs the code and executes the "down" version of the command for all the methods that have it. In the case of adding a column, rollback would remove it.

If you modify your data within the `change` method, ActiveRecord will run that code on both migration and rollback which is probably not what is wanted. Here is an example:

class AddAreaToPlots < ActiveRecord::Migration
  class Plot < ActiveRecord::Base; end

  def change
    add_column :plots, :area, :float

    Plot.find_each do |plot|
      plot.area = plot.width * plot.height
      plot.save!
    end
  end
end

On rollback the migration would first remove the column and then try to populate the column which would of course fail due to the missing column breaking the rollback.

- - -

In addition to the following these guideline it doesn't hurt to understand how the ActiveRecord migrations work. Read the [official Active Record Migrations guide](http://guides.rubyonrails.org/migrations.html) to get more information.

Do you have any best practices you follow to make your migrations better? I'd be interested hear about those in the comments.
