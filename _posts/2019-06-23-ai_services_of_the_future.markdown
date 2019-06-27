---
layout: post
title:      "AI services of the future!"
date:       2019-06-23 09:14:11 -0400
permalink:  ai_services_of_the_future
---


This blog post is going to be about the application that I created to simulate some of the functionality of the SingularityNet network.

What is SIngularityNet anyway? 

SingularityNet is a decentralized marketplace for Artificial Intelligence currently on the Ethereum Network. It is a network where developers can place their AIs into and earn tokens from having other people use their AIs. The currency running this is called the  AGI token. 

 How does this all work? 

Their is a lot of tools that goes into building a new protocol. To name a few Web3.js, Ganache, and Truffle Framework. 

In a later blog post I’ll go deeper on these topics above so stay tooned!
I just want to say a quick note that with this blog post I’ll be talking about how the network works in a broad sense MAJOR ASPECTS OF THE NETWORK ARE LEFT OUT AND CHANGED TO FOR SIMPLICITY! 

WHY tho?

Because why not?! I love both AI and Blockchain so I decided to create a simulated version of the network with a web application using rails.

What did you create already!

So I created a ruby on rails application that demonstrates how the network works in a basic level of course :P. 
The user is able to login/logout/signup. The user is also able to create Ai and Ai services which will then provide a transaction for the user after the user calls a service with an AI. To do this the user has to have a certain amount of data to the user to sign to confirm. 











In this project I am using MVC design pattern (Model–view–controller) which separates different parts of the code which is referred to as Separation of concerns. Which separates different chunks of the code to do a specific job and only that job. The following is an Entity Relationship of the model relationships that I used to connect the models together. 


To give an overview of the application it is best to start from the routes to see the overall flow of the application. The following are the routes for the project:



root 'sessions#home'
Their is a few things going on here:
root is equivalent to the following '/' is the root route. Since position matters in rails this is usually defined first. The way that I used this is by using the Active Record root method. Next their is a convention to display controller actions in your routes.rb which is 'controller#action'. In this case the controller is sessions and the action is home.

Note: Just to make sure we are all on the same page as mentioned in a previous blog post a controller is what communicates between the Model and the view in a MVC architecture.

The following routes are the routes that contribute to login/signup/logout:

get '/signup' => 'users#new'
This is doing a GET request to the /signup route when typing the route into the address bar to retrieve the user’s new view. Ultimately this is signing up a new user. 

post '/signup' => 'users#create'
After the user fills out the form and presses the submit button this will trigger a post request to the signup route in the users controller and hit the user’s create controller action. 


This POST request will be triggered when the user clicks on the submit button on the form.  get the data form the form and putting it inside of params to be sent to the controller in question and in this case it is the create action. 

get '/login' => 'sessions#new'
This is doing a GET request to the /login route. This will login a user that is already in the db.  




post '/login' => 'sessions#create'
This will retreve a request from the /login path to login the user when the user clicks submit which would fire a post request to the sessions create action. 

delete '/logout' => 'sessions#destroy'
Deleting the session which would logout the user

get '/auth/:provider/callback' => 'sessions#create'
This is used for Omniauth which enables users to do the login process through a third party application. The application that I will be using to demonstrate this process is github. The following route ‘/auth/:provider/callback’ is called a callback. Callbacks in the domain of OmniAuth is the return route that the user is redirected to after authentication with all of the users information as a response. Meaning in my case after Github authenticates (confirms that I say who I calm to be) me I would then be redirected to the callback url. Then I would then be sent to the sessions create action to be logged in. 

Next I’ll talk a little about what resources are then I’ll dive into which ones that I used.
I did talk about this in another blog post before but I think it is important to go over it before I continue. As seen in the ruby docs, Restful routes give us a mapping between HTTP verbs(eg. get) and URLs(eg. /signup) to controller actions. By convention, each action also maps to a specific CRUD(create, read, update, delete) operations in a database.

Check out this link to see the resources for my ais and my ai services: 
https://github.com/mfeinLearn/snet-web-simulation/blob/master/Resources%20for%20my%20ais%20and%20my%20ai%20services.pdf

By looking at our Ai and Service model associations we can demonstrate this relationship via our nested routes seen above:

These are my Ai and Service Models:

```
Ai 
   belongs_to :user
   has_many :transactions
   has_many :services, through: :transactions

Service
   has_many :transactions
   has_many :ais, through: :transactions
   has_many :users, through: :ais
```

We can see that we could have ais be nested under services or the other vice versa because both models have many of the other. Later I plan on having transactions to be nested under ais then have services to be nested under transactions. But for specifically for this project I chose to have services to be nested under ais.


 Model
 
The Entity Relationship for this project can be found here:
https://github.com/mfeinLearn/snet-web-simulation/blob/master/erd.pdf


Relationships

The Relationships of the models allow me to interconnect between different db tables to get the data that I want. I used a series of validations, scope methods, and class methods to structure the data to exactly the format that I need it to be to create a better user experience. 

Before I talk about the methods and validations I implemented I think it a good idea to disscus what these are so we are all are on the same page. In the background Active Record is being used.

Scope methods are class methods that are used to retrieving and querying objects. Scope methods return an active record relation object. This will enable us to change scope methods together. Which will enable us to narrow the scope of the query, but if a scope returns nil or false it will return an all scope meaning all of the object in question will be returned.  Scope methods are defined in class which is by default a class method. For example I defined a class method called 
order_by_ais it is defined like the folloiwng:
  scope :order_by_ais, -> {order(balance: :desc)}
Order is an active record sql term that will allow me to order the records. In this example we can order in descending order by the balance of the ai. 




Validations

Validations are used to make sure the data that is saved in your database is what you expect it to be to be able to properly process it in your controller. Their are built in validations that rails creates and developers has an opportunity to build their own as well. I have several validations one in the models and others I have defined in paritals to clean up my code. One validation that I implemented in my code is the following:
validates :name, :description, presence: true
This is an active record validation which is denoted by the validates helper as oppose to validate which is a custom validator.  This validation is making sure that the name and description are present before the object is accepted to be saved the database. Whichever model that is being associated to another table by a belongs_to method is required by default. 

Error Messages

   One cool part about rails is that it allows us to add error messages which can help users to understand what they need to input and in what format. I used error messages in a partial specifically within the layouts directory. In there I defined a partial called _errors.html.erb and added the following code inside of it: 

<ul>
<% object.errors.full_messages.each do |error_message| %>

  <li><%= error_message %></li>

<% end %>
</ul>

Their is an object of errors that we can have access to which enable us to access all of our errors like the following:
@ai.errors. This can allow us to have access to all of the errors that are associated with the @ai. Where the partial comes in is that we can encapsulate this capability within a file called a partial. 

The process of doing this is as follows: 
Add the error message in a file called _errors.html.erb within the layouts directory
Then in the file that has an object that you want to validate you would want to add the following line in the new.html.erb file: <%= render partial: 'layouts/errors', locals: {object: @ai} %>
The word object which is a key in the hash of locals 
In my application the word object will be replaced with the object that you put as the value of object in the locals hash.
To talk about the errors object for a sec it has a method called #full_messages which would return a full message for a given attribute of the object in question.



Controller

The controller is the C in the MVC architecture. 
First I like to talk about the association of objects within the new action. 
In a nested route the creation of the association is in the new action.

The following is an example of the association between the ai and the transaction:

```
class AisController < ApplicationController
def new
  @ai = Ai.new
  @transaction = @ai.transactions.build
  @transaction.build_service
end
end
```

If you look at the relationship between the AI and the Transaction you will see that an ai has many transactions so if we want to create an association on a collection we would need to use the build method to build that association. It would look like this:   
@transaction = @ai.transactions.build
In the same token to associate the transaction with a service you would need to do it like the following: @transaction.build_service.
Where the thing that is being associated to the object in this case the service is appended to the build with an underscore in from. 


The following is an example of the association between the ai and services:
```
class ServicesController < ApplicationController
  def new
    @ai = Ai.find_by_id(params[:ai_id])
    # check if its nested & it's a proper id
    if params[:ai_id] && ai = Ai.find_by_id(params[:ai_id])
    # nested route
      @service = ai.services.build
    else
      #unnested
      @service = Service.new
    end
  end
end 
```

As you can see above the ai is associated to the services similar to how an ai was associated to a transaction but in this case we are associating the ai to services and adding a dot build to build an empty service that is associated to an ai. It looks like the following:       
@service = ai.services.build

 View 
 
The last bit of the MVC architecture that we are going to be talking about is the V in MVC which is the View. If we want to talk about helpers it have pretty cool properties. 
For one it helps take away logic from the views. One would want to put methods in the helpers folder only if it has to deal with displaying something on the views. These helper methods are only used in the views. But their is one limitation which is that one can not use helper methods in the con

Helpers 

I used several helpers to take away code from the views. These helpers are located in a directory called helpers. In here it is strictly for view helpers, that’s it! You can check them out here: https://github.com/mfeinLearn/snet-web-simulation/tree/master/app/helpers

That is a brief summary of my project please checkout the repo for more info!

Last note: This is the MVP of the application I plan on coming back to this and adding a lot more functionality… so stay toon!


