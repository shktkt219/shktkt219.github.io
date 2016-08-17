---
layout: post
title:  "How to Use Devise"
date:   2016-08-17 16:05:17 +0000
---


# About Devise Gem
Devise is a popular authentication solution for Rails application. If you use Devise, you can send confirmation emails, lock user accounts after a certain number of failed login attemppts, send password resets. In addition, Devise can handle to allow multiple models to be signed in, each with different roles, granting different permissions. 



> It's composed of 10 modules:
> 
> * **Database Authenticatable**: hashes and stores a password in the database to validate the authenticity of a user while signing in. The authentication can be done both through POST requests or HTTP Basic Authentication.
> 
> * **Omniauthable**: adds OmniAuth (https://github.com/omniauth/omniauth) support.
> 
> * **Confirmable**: sends emails with confirmation instructions and verifies whether an account is already confirmed during sign in.
> 
> * **Recoverable**: resets the user password and sends reset instructions.
> 
> * **Registerable**: handles signing up users through a registration process, also allowing them to edit and destroy their account.
> 
> * **Rememberable**: manages generating and clearing a token for remembering the user from a saved cookie.
> 
> * **Trackable**: tracks sign in count, timestamps and IP address.
> 
> * **Timeoutable**: expires sessions that have not been active in a specified period of time.
> 
> * **Validatable**: provides validations of email and password. It's optional and can be customized, so you're able to define your own validations.
> 
> *  **Lockable**: locks an account after a specified number of failed sign-in attempts. Can unlock via email or after a specified time period.


That's from the [Devise docs.](http://github.com/plataformatec/devise)

# 1. Add Devise Gem
Open up your `Gemfile` and add this line.

```
 gem 'devise'
```

and run

```
 bundle install
```

to install the gem. 

# 2. Set up  devise in your app
Run the following command in the terminal.

```
 rails g devise:install
```

# 3. Configure Devise
Ensure you have defined default url options in your environments files. Open up `config/environments/development.rb` and add this line.

```
  config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
```

before the `end` keyword.

Open up `app/views/layouts/application.html.erb` and add:

```
  <%= content_tag(:div, flash[:error], :id => "flash_error") if flash[:error] %>
  <%= content_tag(:div, flash[:notice], :id => "flash_notice") if flash[:notice] %>
  <%= content_tag(:div, flash[:alert], :id => "flash_alert") if flash[:alert] %>
```

right above

```
  <%= yield %>
```

# 4. Setup the User model
Now generate your `User` model with:

```
  rails g devise user
	rake db:migrate
```

# 5. Create your first user
Now that you have set everything up you can create your first user. Devise creates all the code and routes required to create accounts, log in, log out, etc.

Open your rails server, create your user account.

# 6. Add sign-up and login links
All we need to do now is to add appropriate links or notice about the user being logged in in the top right corner of the navigation bar.

In order to do thet, edit `app/views/layouts/application.html.erb` add:

```
 <ul class="nav">
  <li class="active"><a href="/ideas">Ideas</a></li>
 </ul>
 
 <p class="navbar-text pull-right">
<% if user_signed_in? %>
  Logged in as <strong><%= current_user.email %></strong>.
  <%= link_to 'Edit profile', edit_user_registration_path, :class => 'navbar-link' %> |
  <%= link_to "Logout", destroy_user_session_path, method: :delete, :class => 'navbar-link'  %>
<% else %>
  <%= link_to "Sign up", new_user_registration_path, :class => 'navbar-link'  %> |
  <%= link_to "Login", new_user_session_path, :class => 'navbar-link'  %>
<% end %>
```

Finally. force the user to redirect to the login page if the user was not logged in. Open up `app/controllers/application_controller.rb` and add:

```
 before_action : authenticate_user!
```




