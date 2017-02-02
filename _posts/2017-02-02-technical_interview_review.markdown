---
layout: post
title:  "Technical Interview Review"
date:   2017-02-02 20:16:49 +0000
---


I took a technical interview, I was nervous and felt difficult. I'd like to review the questions and answers.

*What's a Ruby?*

Ruby is a dynamic, object-oriented programming language which made by Matz in Japan. 

*What's an object-oriented programming?*

It's a programming paradigm based on the concept of "object". It has features of inheritance and encapsulation.

*What is an advantage of inheritance?*

We can reduce development time, and ensures more accurate coding. Inheritance (OOP) is when an object or class is based on another object (prototypal inheritance) or class (class-based inheritance), using the same implementation (inheriting from an object or class) specifying implementation to maintain the same behavior (realizing an interface; inheriting behavior).

*What's class and module?*

Class is the blueprint from which individual objects are created. 
Module is a collection of methods and constants. You can't make an instance of a module.

*What's the difference between include and extend?*

Include is for adding methods to an instance of a class.
Extend is for adding class methods.

*What's active record scope?*

ActiveRecord is a Ruby library for working Relational SQL database like MySQL. It provides an Object Relational Mapping(ORM). Scope adds a class method for retrieving and querying objects. The method is intended to return an ActiveRecord:: Relation object, which is composable with other scopes.

*What's private?*

The method can not be called outside class scope.

*What's a strong parameter?*

It provides an interface for protecting attributes from end-users assignments. This makes Action Controller parameters forbidden to be used in Active Model mass assignment until they have been whitelisted.


*Technical Question
Let’s say you are given an array of n integers, which represents the prices of a stock over n consecutive days. 
*
*Please write a function in the language of your choice that returns the biggest loss you could incur over those n days by buying high and selling low.*

*For example: if the array contains |14|1|18|23|12|8|16|, then the biggest possible loss is $15 (buying at $23 and selling at $8).*

test_1 = [14, 1, 18, 23, 12, 8, 16] # 15
test_2 = [17, 5, 19, 8] # 12

```

def biggest_possible_loss(test)
  max_price = test[0]
  max_loss = test[0] - test[1]
  
  test.each_with_index do |current_price, index|
    if index == 0 then next end
    potential_loss = max_price - current_price
    max_price = [max_price, current_price].max
    max_loss = [max_loss, potential_loss].max
  end
  max_loss
end

```

It was the first time to take a technical interview, I didn't answer perfectly and confidently. I need more practices to explain and to write codes.
