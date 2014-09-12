# Rails-isms

[Kir Shatrov](http://github.com/kirs/), [Evil Martians](http://evilmartians.ru/)

## **Rails-isms** or ActiveRecord-driven-development
!image evilmartians.png
!type is-title

*Kir Shatrov, Evil Martians*

## Kir Shatrov
!type with-em

* [github.com/kirs](https://github.com/kirs)
* [kirs@evilmartians.com](kirs@evilmartians.com)

!image evilmartians.png

## Let's focus on...
## Typical Rails app with **Devise** and **Carrierwave**

##

```ruby
class User < ActiveRecord::Base
  ...
end

user = User.find(42)
user.update(email: "user@example.com")
```

## What typical ORM would do

```sql
UPDATE users SET email='user@example.com'
```

## What it actually does

* writes `user` `unconfirmed_email` attribute
* generates confirmation token with `OpenSSL::HMAC.hexdigest`
* writes the token attribute
* calls mailer → *SMTP* or *Sendgrid REST API*
* saves the model → PostgreSQL query

(actually, a lot MORE callbacks)

## Cat example

```ruby
Photo.create({
  user: user,
  picture_url: "http://grumpy.cat/selfie.jpg"
})
```

## What typical ORM would do

```sql
INSERT INTO photos (user_id, picture_url)
VALUES (1, 'http://grumpy.cat/selfie.jpg')
```

## What it actually does

- downloads the picture locally
- makes a copy for each versions
- calls `imagemagick` for each version
- uploads versions to *Amazon S3* (can take seconds)

## Worse examples

```ruby
class Document < ActiveRecord::Base
  ...
end

pdf = File.open("sample.pdf")
Document.create(file: pdf)
```

## What this code did

* opened PDF with `imagemagick` and `ghostscript`
* splited the document into pages
* converted these pages to JPEG
* created `Document::Image` model with page content
* finally, saved the `Document` to database

## We did something wrong
!type shout

## What to do?

* [7 Patterns to Refactor Fat ActiveRecord Models](http://blog.codeclimate.com/blog/2012/10/17/7-ways-to-decompose-fat-activerecord-models/) by *Code Climate*
* **Sandi Metz** rules
* **Objects on Rails** book

## <s>Fat model, skinny controller</s>
## **ActiveRecord::Base** != center of universe
## **ActiveRecord::Base** != Form object
## **ActiveRecord::Base** != bunch of callbacks
## **ActiveRecord::Base** is just a storage

## What we did

```ruby
class Geoname < ActiveRecord::Base
  acts_as_tree order: :name
  belongs_to :country
end
```

## Plain Old Ruby Objects

common in gems, but not on real apps

## Community responsibility

## To teach
!type shout

## To give good examples
!type shout

## Questions?
!type with-em

* [github.com/kirs](https://github.com/kirs)
* [kirs@evilmartians.com](kirs@evilmartians.com)

!image evilmartians.png
