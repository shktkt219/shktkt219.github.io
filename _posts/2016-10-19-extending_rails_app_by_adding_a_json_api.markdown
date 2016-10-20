---
layout: post
title:  "Extending Rails App by adding a JSON API"
date:   2016-10-19 12:21:18 -0400
---


I've finished my rails app with a jQuery Front End. My app is Meowbox - a website where people can sign up for a monthly subscription for toys and treats for their favorite feline.

My hardest part of this portfolio is Javascript Model Objects part. Implementing the AJAX functionality is using javascript model objects with methods on their prototype to get the ruby objects into javascript before displaying the item resources to the page.

Here is the definition of the item model object:

```
function Item(item){
  this.item_name = item.item_name;
  this.description = item.description;
  this.image = item.image;
  this.size = item.size;
  this.url = item.url;
};
``` 

This allows us to get all of the properties of an item into javascript by passing the item resource sent by the controller in response to the AJAX request for JSON and assigning its properties to a new javascript model object. We can then use the properties of this new javascript model object within our methods on the prototype to help us display the content page. 

When I build out these requirements, I focused on the last line.

Must translate the JSON responses into Javascript Model Objects. The Model Objects must have at least one method on the prototype. Formatters work really well for this.

I decided to use the handlebars format. It was good for me to omit cumbersome handmade codes.

app/assets/javascripts/item.js

```
$(function(){
  Item.templateSource = $("#item-template").html()
  Item.template = Handlebars.compile(Item.templateSource);
})
``` 

app/views/items/new.html.erb

```
<script id="item-template" type="text/x-handlebars-template">
   <h3 class="item_name">{{item_name}}</h3>
   <div class="description">
     <strong>Description : </strong>{{description}}
   </div>
   <div class="image">
     <img src={{image}} height="25" width="25">
   </div>
   <div class="size">
     <strong>Size : </strong>{{size}}
   </div>
   <div class="url">
     <strong><a href="{{url}}">Buy online</a></strong>
   </div>
</script>
```

It's very easy to handle the handlebar, but too simple to use it, so I was confused at first. But I review my former lessons, I've finally created my page.

My javascript codes here.
app/assets/javascripts/item.js


```
function Item(item){
  this.item_name = item.item_name;
  this.description = item.description;
  this.image = item.image;
  this.size = item.size;
  this.url = item.url;
};

$(function(){
  Item.templateSource = $("#item-template").html()
  Item.template = Handlebars.compile(Item.templateSource);
})


Item.prototype.renderLI = function(){
  return Item.template(this);
};

$(function(){
  $("form#new_item").submit(function(event){
    event.preventDefault();
    var values = $(this).serialize();
    var posting = $.post('/items', values);

    posting.done(function(data){
      var item = data["item"];
      var newItem = new Item(item);
      var itemDisplay = newItem.renderLI()
      $("div.new_created_items").append(itemDisplay)
    });
  });
});
```

My github repo is [here](https://github.com/shktkt219/Meowbox_app).

