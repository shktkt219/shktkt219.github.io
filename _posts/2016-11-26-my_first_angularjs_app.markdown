---
layout: post
title:  "My First AngularJS App"
date:   2016-11-26 15:48:11 +0000
---


I've been working on the Flatiron School's Full Stack Web Developer course for about seven months. I've just completed the final project, this is my first app with AngularJS and Rails.

## About my app

For this project, I wanted to create something that I would actually use. I decided to make a Todo List app because I always write a Todo List when I prepare to items for my trip and go shopping.

When you click a get started button, the page will be the dashboard page. You can create new Todo Lists from this form. Do you want to add your new task? If you select a Todo List in All Todo List, the page will be the Todos page. You can add tasks from this form, in addition, you can also sort the tasks which are complete or incomplete. 


<iframe width="560" height="315" src="https://www.youtube.com/embed/mnABVfApv0I" frameborder="0" allowfullscreen></iframe>


This is very simple Todo List page, but it was hard for me to make this. I had two sticking points.

* How to interpret Javascript code to CoffeeScript
* How to add nested status within UI-Router 

## CoffeeScript

I read CoffeeSccript is rails defaults, but I didn't know so much about it. I felt that CoffeeScript was more concise than Javascript, so I decided to use it. At first, I wrote my code in Javascript, and interpreted to CoffeeScript code. I used a compiler [JS2coffee](http://js2.coffee/).

![Imgur](http://i.imgur.com/tFpIz7b.png)

## UI-Router

At first, I used ng-router to my app, but I wated to nested views. So I decided to delete my old code and I added ui-router. 

![Imgur](http://i.imgur.com/0QO8Gpu.png)

I wanted to make index.html a hub page. Home, Dashboard, Todo_lists, About, Help pages all access to index.html at first.


```

#config/routes.rb

root 'templates#index'
get '/dashboard' => 'templates#index'
get '/todo_lists/:id' => 'templates#index'
get '/help' => 'templates#index'
get '/about' => 'templates#index'
get '/templates/:path.html' => 'templates#template', constraints: { path: /.+/ }

```

```

#app/views/templates/index.html.erb

<div class="container">
  <div ui-view></div>
</div>

```

Then, ui-view brows states.

```

#app/assets/javascripts/app.coffee


app = angular.module('todoApp', ['ui.bootstrap', 'ngResource', 'ui.router', 'ngMessages'])

app.config ($stateProvider, $urlRouterProvider) ->

  $urlRouterProvider.otherwise('/home')

  $stateProvider.state('home',
     url: '/home'
     templateUrl: '/templates/home.html'
    ).state('index',
     url: '/'
     templateUrl: '/templates/index.html'
    ).state('index.dashboard',
     url: 'dashboard'
     templateUrl: '/templates/dashboard.html'
     controller: 'DashboardCtrl'
    ).state('index.help',
      url: 'help'
      templateUrl: '/templates/help.html'
    ).state('index.about',
      url: 'about'
      templateUrl: '/templates/about.html'
    ).state('index.todolist',
     url: 'todo_lists/:list_id'
     templateUrl: '/templates/todo_list.html'
     controller: 'TodoListCtrl')

```

And I got nested views!

![Imgur](http://i.imgur.com/r18v0tu.png)
![Imgur](http://i.imgur.com/YcxoPbM.png)
![Imgur](http://i.imgur.com/H0npF7T.png)


My App is very simple but it was great experience for me to think not only AngularJS but also Rails. I will add some function to my app and create more fantasctic app. Thanks.









