---
layout: post
title:      "My Crypto Investor Application"
date:       2019-03-19 14:45:15 -0400
permalink:  my_crypto_investor_application
---


The project that I am working on is called coin investor facts. This is for my love for the crypto community. This is an investment app focused on Cryptocurrency, where a user can create Investment Entries to look up useful information on their investment. 

In this project I used MVC a software architecture pattern that divides an application up into three distinct parts the Model, view and controller. I also used restful routing convention which is basically  a design pattern used for url design which is mapped to different actions  acting on a given resource(is anything - e.g. people, places, or things) and on its entities with in the resource in question. Next, I’ll be talking about my experience with test driven development. Also, I’ll be talking about how I used bcrypt to secure user passwords. Lastly I’ll be talking about sessions and how I used them to enhance the user experience.  

##  Model-View-Controller

Model View Controller or MVC for short is a software architecture that is used to separate a group of similar tasks and bundle them up together into a singular module. These modules are the Model, view, and controllers. Please check out the link above to see how this architecture’s file structure is structured. The job for the model is to handle the business logic by doing queries to the database. The job for the view is to display to the user the resource they want to see. The job for the controller is to act as the intermediary for the view and model. 

The following are the Models that I used: InvestmentEntry, Team, and User. 
The following link is the associations between all of these models: 
https://github.com/mfeinLearn/coin-investor-facts/blob/master/notes/erd.pdf
If you look at the User model you’ll see an attribute of password_digest we will talk about this in a latter section below.

##  Representational State Transfer

Representational State Transfer or Rest comprised of three key components:

> 1) A client-server relationship
> 2) stateless interaction
> 3) Uniform Interface
> 
The client-server model is where the client sends a request using HTTP as a median to send out a GET request to a server. When the server gets the request it send out a responds to the client which results to what you see in your browser which can be a client. So in short Clients are giving the request to the server and the server gets the request then renders it then sends a response back to the client. 

Stateless Interaction is a bit of a complicated topic but the explanation from restfulapi.net made it digestable for me where they talk about that “Stateless Interaction is where the server does not handle anything related to the users state. Client is responsible for storing and handling all application state related information on the client side”.

 It also means that the client is responsible for sending any state information to the server whenever it is needed. 
 
 This essentially means that a user has to send all of their state related information through a request to the server and that the servers job is not to handle user state it has to be sent via the user. But if the server needs state information the client has to provide that to the server. 
 
##  Uniform Interface

The important thing to know is that REST provides a way of mapping HTTP verbs ( get, post, put, delete) and CRUD actions (create, read, update, delete) together. That’s it!
Please note that the CRUD(Creating, Reading, Updating, and Deleting) actions are different actions that occur on the same resource. The following link will show you the restful routes used in this project and what it does: https://github.com/mfeinLearn/coin-investor-facts/blob/master/notes/routes.md
 
A great way to understand this concept is to explain this in the context of this project! 

#### Create Action:
Let's take the example of an investment. When we create an investment an id will be automatically set to it (thanks to activerecord). Let’s say we already had 4 investments already stored in the database and when we create a now investment activerecord sets that to an id of 5.

#### Read Action:
With our investment that we made above we can view the investment, we would make a GET request to /investments/5. 

#### Update Action:
If we want to update the investment for some reason we could change the action to hit that resource to change it. So we can send a PATCH request to /investments/5 to do the update. 

#### Delete Action:
The show page has a form that deletes the investments in question. So when a delete request occurs to the following route this will cause the deletion of the investment from the database by the id which was provided via the url parameters. 

Most importantly for updating and deleting something a sinatra middleware called Rack::MethodOverride was added to the config.ru file.


##  Security

**Security is of course very important to think about when building an application. For this application I am only going to sucrue users passwords but in the future I plan on doing the same thing for all of the rest of the users data. I also use sessions for user authentication**

### Bcrypt

I used the Bcrypt gem for this project. This gem is a hashing algorithm used for securing user passwords. Bcrypt does the following to secure your password:

1 - It takes in your password
2 - Creates a digital fingerprint / hash of your password (password_digest)
3 - By adding in bcrypt to the Gemfile you are then able to use the has_secure_password method which gives us an array of methods to use. One such method is authenticate. authenticate takes in a password which is a plan string and checks it against bcrypts hashing algorithm to make sure it is the correct password. 
4 - In doing the above this allows us to use passwords in our forms and controllers if we want to refer to out password.

If you want to see what this looks like please checkout the repo! XP

#### Sessions

Cookies are files that stores client information for a specific website on a client's computer to enhance the personalized user experience while using the web application. Cookies are sent with requests made to the server of a web applications server. Having to login to authenticate yourself on every page on a given website makes the user experience worse. Sessions makes it so that you do not have to do that on every page. It is important to note that if an application wants a login functionality in their application they must use sessions. 

A session cookie is saved temporary in memory while the user navigates the website. Session cookies are usually deleted after the user logs out from the website. The only distinct factor that distinguishes sessions from other cookies is that it does not have an expiration date. 

#### Test Driven Development

Test Driven Development(TDD) is in its name - doing development driven by creating tests and passing them. 

How to approach TDD:

> 1 - Write the test
> 2 - Make the test fail/error out
> 3 - Write the simplest code that could make the test pass.
> 4 - Refactor to improve.

This method of development quickens development time by a lot compared to mindlessly developing an application with no guidance and assurance that all of your methods are working as expected. 

I created test for the Models and the Controllers in this application to help future developers with maintaining code that works. The test library that I will be using id rspec:
http://rspec.info/

### Conclusion

If you want to checkout the full project: https://github.com/mfeinLearn/coin-investor-facts
Let me know what you think :)
