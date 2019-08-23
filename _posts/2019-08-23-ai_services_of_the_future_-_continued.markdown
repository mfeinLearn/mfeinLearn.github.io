---
layout: post
title:      "AI services of the future - (continued)"
date:       2019-08-23 22:38:35 +0000
permalink:  ai_services_of_the_future_-_continued
---


This is a continuation of the AI services of the future! Blog. This installment I am going to venture in the world of JavaScript. Thinking that now a days their are new machine learning frameworks available that uses javascript like Tensorflow.js and KerasJS to name a few I felt the need to add some javascript into the project. 
In this blog post I will be talking about javascript in the context of a website but in a later post ill be elaborating more on the need for javascript for machine learning. 
Javascript fundamentals

Important javascript concepts before we begin:
What is the DOM? The DOM connect web pages to scripts or programming languages by representing the structure of a document. The DOM is representing a document with a logical tree.

As shown below:
https://en.wikipedia.org/wiki/Document_Object_Model#/media/File:DOM-model.svg

Each branch of the tree ends in a node and each node contains objects. Javascript is used to access the DOM. In the DOM, all HTML elements are defined as objects. 

DOM methods allow programmatic access to the tree. HTML DOM methods are actions you can perform (on HTML Elements). With the DOM methods we can change the document’s structure.

The following are a few examples of DOM methods ( Later in this blog I will show how I implemented them into the project ):

getElementById - returns the element that has the ID attribute with the specified value.

getElementsByTagName - returns a collection of all elements in the document with the specified tag name

createElement - creates an Element Node with the specified name.

appendChild - appends a node as the last child of a node.

What is ajax? Stands for  Asynchronous JavaScript And XML and it can send and receive information. It can communicate with the server, exchange data, and update the page without having to refresh the page. The two major characteristics of AJAX is that it makes requests to the server without reloading the page and it receives and work with data from the server. 

What is JSON? It is a lightweight format that uses human-readable text for storing and transporting data.

What is JQuery? Is a javascript library that selects (query) HTML elements and perform "actions" on them.

What is the fetch api? The fetch API acts just like XMLHttpRequest (XHR) which can retrieve data/resources from a URL and communicates with a server from a URL without having to refresh a page. 

What is a JavaScript Model Object? A JavaScript Model Object is a way to create javascript objects. 
The way to build them is by getting JSON responses and converting them into javascript objects. In my application I used a constructor to build a service and an ai javascript objects. 


### The application:
##### Index of services
What I decided to do to render the services index page via JavaScript & JSON is to hijack the All Service button in the navbar which originally rendered by rails but I added an event listener on the button so when a user clicks it that would feach json from my backend specifically the http://localhost:3000/services endpoint which returned a response of a list of services in json format. 

With the json I serialized it in my rails backend which created services objects. This allowed me to append the result onto the dom. 

This is done in the following steps:
The click event from an event listener is triggered. Specifically on the all_services id which is located on the navbar. 
We have to prevent the traditional rails response to occur and instead use the fetch API to fetch the /services resource which returns a promise that needs to be handled by the then method ( Fetches are thenable meaning it has a then method. You cannot access the data right after you call it, this is due to the fact it is a Promise. One can only access the data after a then. After that we need to convert our response Promise to a JSON promise. This is done by the following: res.json(). This will enable us to get access to the data after the last then ) 
From there, we can manipulate the data in this case it is a collection of services. 
We can now iterate through that collection to create a collection of “honest to god” service objects. Which is created with a constructor in our case a service constructor. 
I take that service object and format the HTML for the index page by creating a formatter method on the service prototype method. 
 From their I append it to the blank dom which repaints the dom with my newly formatted index page with services. 

##### Show page of a specific service

I also decided to render all of the service’s link on the services index pages that is rendered via javascript AKA render a service show page via JavaScript & JSON. 
When a user clicks on one of those links an event listener is triggered which would fetch a specific service from my backend specifically at the http://localhost:3000/services/:id.json endpoint. :id meaning the id of the specific service. 

The click event from an event listener is triggered. Specifically on the show_link class which is located on each service object on the javascript index page of services. 
We have to prevent the traditional rails response to occur and instead use the fetch API to fetch the /services/:id.json resource. The process is the same as above.
From there, we can manipulate the data in this case it is a specific service json string which then can be converted into a “honest to god” service object. Which is created with a constructor in our case a service constructor. 
I take that service object and format the HTML for the show page by creating a formatter method on the service prototype method. 
From their I append it to the blank dom which repaints the dom with my newly formatted show page with a service.


##### Displaying a Service ais 


Next I decided to add a service ais (meaning a service has many ais) on that services show page via JavaScript & JSON. I iterated through it’s ais then I appended the ai’s id, name, and description attributes to the dom as well. 

I did the following to accomplish this:

How I created a has many relationship was that I iterated through the services ais (a service has many ais) then did the same as I have done which is:
manipulate the data in this case it is a specific ai json string which then can be converted into a “honest to god” ai object. Which is created with a constructor in our case a ai constructor. 
I take that ai object and format the HTML for the show page by creating a formatter method on the ai prototype method. 
From their I append it to the dom with its associated service on the newly formatted show page.

##### Form submitting via AJAX


The form that I used to submit via AJAX is the form to create a new service. The steps that this is done is as follows:
We input the contents for a service (name, description, and price) then when we press submit a sequence occurs. 
The form where a user can input data has an id of new_service.
Since it is a form that we want to listen for an event we add a listener on the “submit” event. 
I prevented the default event. Essentially, I just want the form submission to pause automatically to hijack the submission of this particular service.
Next I want to grab the values that I input into this form. I did that with jQuary with a method called serialize. This is done by the following - a) wrapping the this (which is in this case the form in a jquary object) in parenticies and call the jQuary on it like the following: $(this). 
Then I call the rails method serialize on it (serialize()) (this serialize the information that I put into the form and will allow us to then send back to our server)
We can take that serialized data and use that to post to my rails backend.
I can post to my rails back end by using jQuery to do a post request to /services ( With of course our serialized data which is passed in. )
once we are successful with posting we can chain a method called .done which takes in a callback function that gives us the data that is returned to us from the server
then I can repaint the dom how ever I see fit
 now we replaced everything inside that div with whatever we want.

Pleasr Note: I will be updating this blog so stay tuned... like Looney Tunes 😁😎


