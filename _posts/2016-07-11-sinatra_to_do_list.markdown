---
layout: post
title:  "Sinatra To Do List"
date:   2016-07-11 19:25:53 +0000
---


I tried to create an app for Sinatra final project. I wanted it to be something that I would be a user. I finally decided that I would do some form of to-do list app because I always use a to-do list.

This app allows users to create an account and then add tasks and due dates. The user is able to view all of the to-do lists and update all information about the list. In addition, he/she can delete everything contents. I have a user model and a list model. A user has many lists, list belongs to a user. 

The hardest part of this whole application for me was understanding the contents of the database. At first, I confused how many users do I have, but I used the tux gem I could clearly understand. 

I stuck with bugs, for example, I wrote a code "get '/lists/new' do...." but Sinatra app said, please write a route "get '/lists/new' do...". The other one, when I added the last code with wrapping "end" in "application_controller.rb", the app said, "It's a syntax error!!" I recreate my repositories for three times. This project was tough for me but I can understand Sinatra  application deeply. 

Thank you for taking the time for reading this post.
