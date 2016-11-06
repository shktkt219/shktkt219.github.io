---
layout: post
title:  "Extending Rails App by adding a JSON API (new Item show feature)"
date:   2016-11-06 06:04:39 +0000
---


In a live cording  of Rails App with a jQuery Front End, I tried to add new Item feature to box show pages in my meowbox app. 

![](http://i.imgur.com/SZvKz1C.png)

My goal is simple. When you click a link which an item's show page, item's details will appear in the same page without page refresh. For instance, when you click the link of a hello cap in the item list, the item details will appear in the same page.

At first, add "item-tag" class in the item link.

```

<ul>
    <% @box.items.each do |item| %>
			 <li>
				 <%= link_to item.item_name, box_item_path(@box, item), class: "item-tag"%> | 
				 <%= item.size %>
			 </li>
    <% end %>
  </ul>
	
```

Then, add a script tag for writing AJAX codes in the end of this page and $(function(){};, this is the same feature $(document).ready(function(){};  for waiting for the page to load before running our AJAX request. But if you write javascript code in the same page, you don't have to write it.

```

<script type="text/javascript" charset="utf-8">
  $(function(){
	
	});
</script>

```

Select item-tag class and add event listener .on("click", function(event){};. $('.item-tag') is jQuery objects and in the console, it look like this.

![](http://i.imgur.com/BBOkBAB.png)

```

<script type="text/javascript" charset="utf-8">
  $(function(){
	   $('.item-tag').on('click', function(event){
		   event.preventDefault(); 
		 });
	});
</script>

```

event.preventDefault(); is very important line. In default, when you click an item link, it will happen page refresh. However, if you have a line of event.preventDefault();, you can click an item link without page refresh.

Next, I want to grab the url. This case, the route is href attribute in a tag. 

![](http://i.imgur.com/0crKiAx.png)

So, my code is below.

```

<script type="text/javascript" charset="utf-8">
  $(function(){
    $('.item-tag').on("click", function(e){
      e.preventDefault();
      var tag = $(this)
      $.get(tag.attr("href") + ".json", function(data){
        
      });
    });
  });
</script>

```

The first parameter is the URL with the data, tag.attr("href") + ".json" is the code for getting json data in the AJAX get request.

![](http://i.imgur.com/JWTF5FH.png)

The second parameter is a function to handle the response. We are getting the element on the page with the id of sentences and inserting the response.

```

<div id="resultItem">
  <h3 id="resultItemName"></h3>
  <p id="resultItemDescription"></p>
</div>

```

```

<script type="text/javascript" charset="utf-8">
  $(function(){
    $('.item-tag').on("click", function(e){
      e.preventDefault();
      var tag = $(this)
      $.get(tag.attr("href") + ".json", function(data){
        var item = data["item"]
        $("#resultItemName").text(item["item_name"]);
        $("#resultItemDescription").text(item["description"]);
      });
    });
  });
</script>

```


Now, if you click hello cap link, the details of hello cap will appear in the same page without page refresh. 

![](http://i.imgur.com/qJeKsU2.png)












