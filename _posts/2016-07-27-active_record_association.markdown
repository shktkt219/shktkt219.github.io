---
layout: post
title:  "Active Record Association"
date:   2016-07-27 17:57:18 -0400
---


** Why Associations?**

Do you know the reason why do we need associations between models? That's because they make common operations simpler and easier in our code. For instance, there is a simple application that includes a model for customers and a model for orders. Each customer can have many orders. Without associations, the model would look like this.

```

  class Customer < ActiveRecord::Base
  end

  class Order < ActiveRecord::Base
  end
  
```


When we want to add a new order for an existing customer, we will need to add some codes like this.

```

  @order = Order.create(order_date: Time.now, customer_id: @customer.id)  
  
```


Or when we try to delete a customer, we should ensure that all of its orders get deleted as well.


```

  @orders = Order.where(customer_id: @customer.id)
  @orders.each do |order|
      order.destroy
  end
  @customer.destroy

```

With Active Record associations, we can streamline some operations by declaratively telling an application that is a connection between the two models. This is the revised code for setting up an association.

```

  class Customer < ActiveRecord::Base
      has_many :orders, dependent: :destroy
  end

  class Order < ActiveRecord::Base
      belongs_to :customer
  end

```

With this change, it will be easier to create a new order for a particular customer.

```
  @order = @customer.orders.create(order_date: Time.now )
```

Deleting a customer and all of its orders is easier too.

```
  @cutomer.destroy
```

There are the different types of associations.


* belongs_to
* has_one
* has_many
* has_many :through
* has_one :through
* has_and_belongs_to_many


In order to implement these relationships we will need to do two things:


1. Write a migration that creates tables with associations. For example, if a dog belongs to an owner, the dogs table should have an *owner_id* column.
2. Use Active Record macros in the models.


This time, taking a closer look at belongs_to, has_many, and has_many :through associations.


**Create a music playing app**

We'll be building a domain model for a music playing app. This app has three models: Artists, Songs, and Genres. The relationships between artists, songs, and genres will be following:

* Artist have many songs and a song belongs to an artist.
* Artist have many genres through songs.
* Songs belongs to a genre.
* A genre has many songs.
* A genre has many artists through songs.

Migration files like this.


*db/migrate/01_create_songs.rb*

```
  class CreateSongs < ActiveRecord::Migration
    def change
      create_table :songs do |t|
        t.string :name
        t.integer :artist_id
        t.integer :genre_id
      end
    end
  end    
```


*db/migrate/02_create_artists.rb*

```
  class CreateArtists < ActiveRecord::Migration
    def change
      create_table :artists do |t|
        t.string :name
      end
    end
  end
```


*db/migrate/03_create_genres.rb*

```
  class CreateGenres < ActiveRecord::Migration
    def change
      create_table :genres do |t|
        t.string :name
      end
    end
  end
```

After running *rake db:migrate* in your terminal, create files for models. Define all classes to inherit from *ActiveRecord::Base*. We will be using the following ActiveRecord macros.

* has_many
* has_many through
* belongs_to



*song.rb*

```
  class Song < ActiveRecord::Base
    belongs_to :artist
    belongs_to :genre
  end   
```


*artist.rb*

```
  class Artist < ActiveRecord::Base
    has_many :songs
    has_many :genres, through: :songs
  end   
```


*genre.rb*

```
  class Genre < ActiveRecord::Base
    has_many :songs
    has_many :artists, through: :songs
  end    
```


**Code in action**

Our associations are working because of our migrations and use of AR macros. Playing around with our code in *rake console*.

Making a few new songs.

![](http://i.imgur.com/QFEVzFP.png)

Now, we have two songs. Next, we will make some artists to associate them to.

![](http://i.imgur.com/cmnGOnS.png)

An individual song has an *artist_id*. AR macros we implemented in our classes allow us to associate a song object directly to an artist object:

![](http://i.imgur.com/1UCjn3n.png)

We can do the same for *dont_mind* and *kent_jones*.

![](http://i.imgur.com/SjM3Vm4.png)

We can also ask our artists what songs they have. Making the second song for *daichi_miura*.

![](http://i.imgur.com/JTOhhjQ.png)

But, when we ask *daichi_miura* for his songs, his collection of songs will be empty. That's because the model that *has_many* is considered the parent. The model that *belongs_to* is considered the child. If we tell the child that it belongs to the parent, **the parent won't know about that relationship.** If we tell the parent that a certain child object has been added to its collection, **both the parent and the child will know about the association.**

Let's see this with creating another new song and add it to *daichi_miura*'s songs collection:

![](http://i.imgur.com/EyI5I5f.png)

We added *cry_and_fight* to *daichi_miura*'s collection of songs and we can see the *daichi_miura* knows it has that song in the collection and *cry_and_fight* knows about its artist.

*daichi_miura.songs* returns an array of songs. When a model *has_many* of something, it will store those objects in an array.

Let's add some genres to our has many through association.

![](http://i.imgur.com/4fKcDdO.png)
![](http://i.imgur.com/23N4RIv.png)

Our AR association is working.













