---
layout: post
title: Patterns in ruby and rails
date: 2021-02-27
tags:
  - ruby
keywords:
  - patterns rails
  - patterns ruby
  - design patterns
dont_show_excerpt_separator: false
---

Some patterns here for reference:

### Value Object

Probably the simplest pattern is Value Object - PORO(plain old ruby object) that provides methods that return values:
```ruby
class User
  def initialize (full_name:, email:)
    @full_name = full_name
    @email = email
  end

  attr_reader :full_name, :email

  def first_name
    full_name.split(" ").first
  end

  def last_name
    full_name.split(" ").last
  end
end
```

<!--more-->

### Service

Service object is responsible for doing only one thing:

```ruby
class Greeter
  def self.call(name, title)
    "Welcome back #{title}. #{name}!"
  end
end
```
Of course, in reallity, you will have more complicated logic, but you want to make your service responsible for one thing only (single responsibility rule).

### Decorator
Honestly, I am confusing `decorators` and `presenters`. I found this [explanation and example here](https://longliveruby.com/articles/rails-design-patterns-the-big-picture):
> The decorator pattern is similar to the presenter, but instead of adding additional logic, it alters the original class's behavior.

> We have the Post model that provides a content attribute that contains the post’s content. On the single post page, we would like to render the full content, but on the list, we would like to render just a few words of it:

```ruby
class PostListDecorator < SimpleDelegator
  def content
    model.content.truncate(50)
  end

  def self.decorate(posts)
    posts.map { |post| new(post) }
  end

  private

  def model
    __getobj__
  end
end

@posts = Post.all
@posts = PostListDecorator.decorate(@posts)
```

### Presenter

This pattern is used when you need to isolate some logic and is usually (not always) used in views.

```ruby
class UserPresenter
  def initialize(user)
    @user = user
  end

  def role
    if @user.admin?
      'Administrator'
    elsif @user.signed_in
      'User'
    else
      'Guest'
    end
  end
end
```

# Policy Objects

These objects are responsible for permissions, policies etc. Examples of gems will be `cancan`, `pundit`

```ruby
class UserAccoutnPolicy
  def initialize(user, account)
    @user = user
    @account = account
  end

  def access?
    admin? && @account.user == @user
  end

  def close?
    access? && @account.active? && @user.balance.positive?
  end

  private

  def admin?
    @user.role == 'admin'
  end
end
```

or another example

```ruby
class BankTransferPolicy
  def self.allowed?(user, recipient, amount)
    user.account_balance >= amount &&
      user.transfers_enabled &&
      user != recipient &&
      amount.positive?
  end
end
```

### Builder

Again, nice example with file parsers from [long live ruby](https://longliveruby.com/articles/rails-design-patterns-the-big-picture)
> The builder pattern is often also called an adapter. The pattern’s main purpose is to provide a simple way of returning a given class or instance depending on the case. If you are parsing files to get their contents you can create the following builder:

```ruby
class FileParser
  def self.build(file_path)
    case File.extname(file_path)
      when '.csv' then CsvFileParser.new(file_path)
      when '.xls' then XlsFileParser.new(file_path)
      else
        raise(UnknownFileFormat)
      end
  end
end

class BaseParser
  def initialize(file_path)
    @file_path = file_path
  end
end

class CsvFileParser < BaseParser
  def rows
    # parse rows
  end
end

class XlsFileParser < BaseParser
  def rows
    # parse rows
  end
end
```
And usage:
you can access the rows without worrying about selecting a good class that will be able to parse the given format:

```ruby
parser = FileParser.build(file_path)
rows = parser.rows
```

### Null object

Nobody likes to deal with `nil`s and having those extra guards and defensive style.
If you expect a user in your code, and user is not created (for example guest), rather than check for `nil` create `GuestUser`

```ruby
class GuestUser
  def name
    'Guest'
  end

  def posts
    []
  end
end
```

Now if you need to perform something with user you don't need to check for `nil`s and can treat user as it always exists.

```ruby
user = current_user || GuestUser.new
puts "Welcome #{user.name}"
puts "Number of posts created #{user.posts.size}"
```

## Afterword

Patterns solve problems but when used incorrectly they bring unneeded complexity to your codebase.