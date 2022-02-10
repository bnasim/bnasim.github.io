---
title: "Using Retrofit with Kotlin in Android"
date: 2022-02-08T15:34:30-03:00
categories:
  - blog
tags:
  - Kotlin
  - Retrofit
---


 I will discuss Android concepts based on an app/project. So we are going to build an app called MyNews. The description of the project is as follows.


# Functional Description


 We will use the news published by the New York Times and will display it within the app. The[ New York Times has an API](https://developer.nytimes.com/)  which allows users to get all the articles they publish and even their film and book reviews, most-shared articles, and more. We will take advantage of the following functionalities: 



* The top articles at any moment (via the [Top Stories API](https://developer.nytimes.com/docs/top-stories-product/1/overview)).
* The most popular articles (via the [Most Popular API](https://developer.nytimes.com/docs/most-popular-product/1/overview)).
* The articles that interest you (via the [Article Search API](https://developer.nytimes.com/docs/articlesearch-product/1/overview)).

 Mock up of the initial app design looks like this.
    

![alt_text](/assets/images/image1.png "home screen")

The application will have several tabs:

* Top Stories.
* Most Popular.
* Subjects that interest you (Business, Sports and Arts, etc). Feel free to add as many subjects as you like.

When you click on an article, the article should be shown in a WebView.

A magnifying glass icon should be displayed, and when pressed, it should allow you to search all articles in the New York Times database (not just the articles currently shown on the screen). Here's the search screen to come up with:


![alt_text](/assets/images/image2.png "search screen")



## Notifications:

We will add notification functionality so you don't miss any important articles! Notifications will alert you when new articles are published depending on the criteria we will have predefined. To do this, we will add a notifications settings area that's accessible via the application home screen. 


![alt_text](/assets/images/image3.png "notification screen")


![alt_text](/assets/images/image4.png "notification")


I will split the requirements into mini tasks. I will use Android MVVM(Model View ViewModel) to architecture the app. 

So the tasks are

* Getting data from New York Times API, loading that in RecyclerView.
* Fetch image from API and display in RecyclerView.
* DiffUtil and data binding with RecyclerView.
* Add recyclerView in Activity or Fragment
* Apply MVVM design pattern.

       ViewModel & ViewModelProvider
       LiveData & Livedata Observers
       DataBinding with ViewModel & LiveData.
       
* Retrofit with Coroutine
* Retrofit without Coroutine
* Add Repository
* Navigation graph
* Add Room database.
* Search Functionality
* Notification
* Unit test and Integration tests
* Continuous Integration	


