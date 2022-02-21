---
title: "Get data from News API and show it in RecyclerView"
date: 2022-02-21T15:34:30-03:00
categories:
  - blog
tags:
  - Kotlin
  - Retrofit
  - Android
---


# Getting data from New York Times API, loading that in RecyclerView.


 We will use Retrofit to get data from the New York Times API.


 Retrofit is **a type-safe REST client for Android, Java and Kotlin** developed by Square. The library provides a powerful framework for authenticating and interacting with APIs and sending network requests with OkHttp. ... This library makes downloading JSON or XML data from a web API fairly straightforward. Retrofit turns HTTP API into Java/Kotlin interface. You can know more about Retrofit from here.


[https://square.github.io/retrofit/](https://square.github.io/retrofit/)


# steps to create a retrofit android example   

* Add the retrofit dependency
* Creating Model Class
* Create The Retrofit Instance
* Setting Up The Retrofit Interface
* Consume The REST Web Service
* Set Retrofit response data into the Recyclerview

# Add the retrofit dependency   

 First, Add Internet Permission to your Application in  AndroidManifest.xml 


```
<uses-permission android:name="android.permission.INTERNET"></uses-permission>
```


 Then, We have to add the retrofit dependency into our build.grade file.   We can find the latest retrofit version on the official retrofit website. <code>[http://square.github.io/retrofit/](https://square.github.io/retrofit/)</code>


```
implementation 'com.squareup.retrofit2:retrofit:2.9.0'
```


And the GSON converter from retrofit used to convert the JSON response from the server.


```
 implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
```

# Creating Model Class
    

You need to signup with the New York Times to access their API from here.[https://developer.nytimes.com/](https://developer.nytimes.com/)

 To get Top Stories articles, the example call is

[https://api.nytimes.com/svc/topstories/v2/arts.json?api-key=yourkey](https://api.nytimes.com/svc/topstories/v2/arts.json?api-key=yourkey)

Before creating the model, you need to know what type of response we will be receiving. You can try New York Times API from their site from here

[https://developer.nytimes.com/docs/top-stories-product/1/routes/%7Bsection%7D.json/get](https://developer.nytimes.com/docs/top-stories-product/1/routes/%7Bsection%7D.json/get)





![alt_text](images/image5.png "image_tooltip")


I will use a plugin to convert this response to the data models. Please install the plugin “Kotlin data class file from Json”. Paste json response as shown in picture, it will create data models for you.


![alt_text](images/image6.png "image_tooltip")


So these are the data models created for us.


```
data class ResponseNews(
   val copyright: String,
   val last_updated: String,
   val num_results: Int,
   val results: List<Result>,
   val section: String,
   val status: String
)
```
```
data class Result(
   val `abstract`: String,
   val byline: String,
   val created_date: String,
   val des_facet: List<String>,
   val geo_facet: List<String>,
   val item_type: String,
   val kicker: String,
   val material_type_facet: String,
   val multimedia: List<Multimedia>,
   val org_facet: List<String>,
   val per_facet: List<String>,
   val published_date: String,
   val section: String,
   val short_url: String,
   val subsection: String,
   val title: String,
   val updated_date: String,
   val uri: String,
   val url: String
)
```
```
data class Multimedia(
   val caption: String,
   val copyright: String,
   val format: String,
   val height: Int,
   val subtype: String,
   val type: String,
   val url: String,
   val width: Int
)
```



## Create The Retrofit Instance:

We need to create the Retrofit instance to send the network requests. we need to use the Retrofit Builder class and specify the base URL for the service.

ApiInterface.kt code
```
interface ApiInterface {
    @GET("/svc/topstories/v2/arts.json?api-key=ueIaGytOq58AhcnJefgEvSvWrOOH9ozi")
    suspend fun getAllArticles(): Response<ResponseNews>
}
```


## Setting Up The Retrofit Interface

​​Retrofit provides the list of annotations for each HTTP methods:

@GET, @POST, @PUT, @DELETE, @PATCH or @HEAD.

The endpoints are defined inside of an interface using retrofit annotations to encode details about the parameters and request method. T return value is always a parameterized Call&lt;T>.

Because the POJO classes are wrapped into a typed Retrofit Call class.

Method Parameters :

@Body — Sends Java objects as the request body.

@Url — use dynamic URLs.

@Query — We can simply add a method parameter with @Query() and a query parameter name, describing the type.


## Consume The REST Web Service

All the setups are done. Now we are ready to consume the REST web service. In Our MainActivity.Java, First, need to initialize the ApiClient.


## Set Retrofit response data into the recyclerview


### RecyclerView Android Dependency

For using RecyclerView in your project you need to add the recycler view support library to your project. Add the following gradle dependency to project **build.gradle** file.


```
dependencies {
   implementation "androidx.recyclerview:recyclerview:1.1.0"
}
```



### Create layout with RecyclerView

Add android Recyclerview into **activity_main.xml**

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

        <androidx.recyclerview.widget.RecyclerView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/recyclerView"/>


</RelativeLayout>
```

Prepare adapter for RecyclerView.

```
class RvAdapter(val articles: List<Result>): RecyclerView.Adapter<ArticlesViewHolder>() {
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ArticlesViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.item_layout, parent, false)
        return ArticlesViewHolder(view)
    }

    override fun getItemCount(): Int {
        return articles.size
    }

    override fun onBindViewHolder(holder: ArticlesViewHolder, position: Int) {
        return holder.bind(articles[position])
    }
}

class ArticlesViewHolder(itemView : View): RecyclerView.ViewHolder(itemView){
   // private val photo:ImageView = itemView.findViewById(R.id.movie_photo)
    private val publish:TextView = itemView.findViewById(R.id.textViewPublishDate)
    private val section:TextView = itemView.findViewById(R.id.textViewSection)
    private val title:TextView = itemView.findViewById(R.id.textViewTitle)
  //  private val rating:TextView = itemView.findViewById(R.id.movie_rating)

    fun bind(article: Result) {
     //   Glide.with(itemView.context).load("http://image.tmdb.org/t/p/w500${movie.poster_path}").into(photo)
        publish.text = article.published_date
        section.text = article.section
        title.text = article.title
     //   rating.text = "Rating : "+movie.vote_average.toString()
    }

}
```


Set RecyclerView adapter to RecyclerView.

```
class MainActivity : AppCompatActivity() {

    lateinit var txtData: TextView
    lateinit var recycleData: RecyclerView
    private lateinit var adapter1: RvAdapter
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
      //  txtData = findViewById(R.id.txtData)
        recycleData = findViewById(R.id.recyclerView)
        recycleData.layoutManager = LinearLayoutManager(this)
        recycleData.addItemDecoration(
            DividerItemDecoration(
                recycleData.context,
                (recycleData.layoutManager as LinearLayoutManager).orientation
            )
        )


                    getArticleList()
    }

    fun getArticleList() {
        var retrofit = RetrofitClient.getInstance()
        var apiInterface = retrofit.create(ApiInterface::class.java)
        lifecycleScope.launchWhenCreated {
            try {
                val response = apiInterface.getAllArticles()
                if (response.isSuccessful()) {
                    var json = Gson().toJson(response.body())

                 //   recycleData.layoutManager = LinearLayoutManager()
                    adapter1 = RvAdapter(response.body()!!.results)
                     //   txtData.setText(response.body().results.get(0).section)
                    recycleData.adapter = adapter1
                    }

                    //new
                   /* if(response?.body()!!.support.text.contains("Harshita")){
                        Toast.makeText(
                            this@MainActivity,
                            "Hello Retrofit",
                            Toast.LENGTH_LONG
                        ).show()
                    }*/

                   // var getNEsteddata=response.body().data.get(0).suport.url

                 else {
                    Toast.makeText(
                        this@MainActivity,
                        response.errorBody().toString(),
                        Toast.LENGTH_LONG
                    ).show()
                }
            }catch (Ex:Exception){
                Log.e("Error",Ex.localizedMessage)
            }
        }

    }

}
```


