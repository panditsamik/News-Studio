# News-Studio

News Studio is an Android news app built using Kotlin.

It requests news from <https://saurav.tech/NewsAPI/top-headlines/category/general/in.json> with the help of Volley library.

Used Glide as image loading framework.

Customs tabs are used to display the web content to users with several customizations.

## Link to the API
<https://saurav.tech/NewsAPI/top-headlines/category/general/in.json>

# Screenshots

![1](https://user-images.githubusercontent.com/91545371/192874332-fd2daafd-efe0-48fa-9fd4-cbb2d1b04910.jpeg)

![2](https://user-images.githubusercontent.com/91545371/192874362-1105498b-14d7-4b61-af29-4d0bd0f2e4b0.jpeg)

![3](https://user-images.githubusercontent.com/91545371/192874389-3800dbad-87e0-4b57-bdac-928d5af2becb.jpeg)

# Video

https://user-images.githubusercontent.com/91545371/192874544-a8753a1e-2a0e-44a5-897b-1368740b3993.mp4

## Volley Overview

Volley is an HTTP library that makes networking for Android apps easier and most importantly, faster.

### Send a Simple Request

To use Volley, you must add the<android.permission.INTERNET>
permission toyour app’s manifest. Without this, your app won’t be able to connect to the network.

Add Volley to your project.To add the following dependency to your app’s build.gradle file:

```
dependencies {
    implementation("com.android.volley:volley:1.2.1")
}
```

### Use newRequestQueue

Kotlin

```
val textView = findViewById<TextView>(R.id.text)
// ...

// Instantiate the RequestQueue.
val queue = Volley.newRequestQueue(this)
val url = "https://www.google.com"

// Request a string response from the provided URL.
val stringRequest = StringRequest(Request.Method.GET, url,
        Response.Listener<String> { response ->
            // Display the first 500 characters of the response string.
            textView.text = "Response is: ${response.substring(0, 500)}"
        },
        Response.ErrorListener { textView.text = "That didn't work!" })

// Add the request to the RequestQueue.
queue.add(stringRequest)
```

Java

```
final TextView textView = (TextView) findViewById(R.id.text);
// ...

// Instantiate the RequestQueue.
RequestQueue queue = Volley.newRequestQueue(this);
String url = "https://www.google.com";

// Request a string response from the provided URL.
StringRequest stringRequest = new StringRequest(Request.Method.GET, url,
            new Response.Listener<String>() {
    @Override
    public void onResponse(String response) {
        // Display the first 500 characters of the response string.
        textView.setText("Response is: " + response.substring(0,500));
    }
}, new Response.ErrorListener() {
    @Override
    public void onErrorResponse(VolleyError error) {
        textView.setText("That didn't work!");
    }
});

// Add the request to the RequestQueue.
queue.add(stringRequest);
```
### Use a singleton pattern

Kotlin

```
class MySingleton constructor(context: Context) {
    companion object {
        @Volatile
        private var INSTANCE: MySingleton? = null
        fun getInstance(context: Context) =
            INSTANCE ?: synchronized(this) {
                INSTANCE ?: MySingleton(context).also {
                    INSTANCE = it
                }
            }
    }
    val imageLoader: ImageLoader by lazy {
        ImageLoader(requestQueue,
                object : ImageLoader.ImageCache {
                    private val cache = LruCache<String, Bitmap>(20)
                    override fun getBitmap(url: String): Bitmap {
                        return cache.get(url)
                    }
                    override fun putBitmap(url: String, bitmap: Bitmap) {
                        cache.put(url, bitmap)
                    }
                })
    }
    val requestQueue: RequestQueue by lazy {
        // applicationContext is key, it keeps you from leaking the
        // Activity or BroadcastReceiver if someone passes one in.
        Volley.newRequestQueue(context.applicationContext)
    }
    fun <T> addToRequestQueue(req: Request<T>) {
        requestQueue.add(req)
    }
}
```

Java

```
public class MySingleton {
    private static MySingleton instance;
    private RequestQueue requestQueue;
    private ImageLoader imageLoader;
    private static Context ctx;

    private MySingleton(Context context) {
        ctx = context;
        requestQueue = getRequestQueue();

        imageLoader = new ImageLoader(requestQueue,
                new ImageLoader.ImageCache() {
            private final LruCache<String, Bitmap>
                    cache = new LruCache<String, Bitmap>(20);

            @Override
            public Bitmap getBitmap(String url) {
                return cache.get(url);
            }

            @Override
            public void putBitmap(String url, Bitmap bitmap) {
                cache.put(url, bitmap);
            }
        });
    }

    public static synchronized MySingleton getInstance(Context context) {
        if (instance == null) {
            instance = new MySingleton(context);
        }
        return instance;
    }

    public RequestQueue getRequestQueue() {
        if (requestQueue == null) {
            // getApplicationContext() is key, it keeps you from leaking the
            // Activity or BroadcastReceiver if someone passes one in.
            requestQueue = Volley.newRequestQueue(ctx.getApplicationContext());
        }
        return requestQueue;
    }

    public <T> void addToRequestQueue(Request<T> req) {
        getRequestQueue().add(req);
    }

    public ImageLoader getImageLoader() {
        return imageLoader;
    }
}
```
### RequestQueue operations using the singleton class

Kotlin

```
MySingleton.getInstance(this).addToRequestQueue(stringRequest)
```


Java

```
MySingleton.getInstance(this).addToRequestQueue(stringRequest);
```

#### Form more information

<https://google.github.io/volley/requestqueue.html>

## Glide Overview

Glide is a fast and efficient open source media management and image loading framework for Android that wraps media decoding, memory and disk caching, and resource pooling into a simple and easy to use interface.Glide supports fetching, decoding, and displaying video stills, images, and animated GIFs. Glide includes a flexible API that allows developers to plug in to almost any network stack.

## Use Gradle

```
repositories {
  google()
  mavenCentral()
}

dependencies {
  implementation 'com.github.bumptech.glide:glide:4.13.2'
  annotationProcessor 'com.github.bumptech.glide:compiler:4.13.2'
}
```
### For more information on Glide

<https://github.com/bumptech/glide>

## Custom Tabs Overview 

Custom Tabs is a browser feature, introduced by Chrome,
that is now supported by most major browsers on Android. 
It give apps more control over their web experience, and
make transitions between native and web content more seamless 
without having to resort to a WebView.

###  Add the library dependency

Navigate to the Gradle Scripts > build.gradle(Module: app), add the library in the dependencies section, and sync the project.

```
dependencies {
      implementation 'androidx.browser:browser:1.3.0'
}
```

### Working with the MainActivity.kt file

```
override fun onItemClicked(item: News) {
        val builder =  CustomTabsIntent.Builder()
        val customTabsIntent = builder.build()
        customTabsIntent.launchUrl(this, Uri.parse(item.url))
    }
```


# Developer

Reach out to me at

<https://www.linkedin.com/in/samik-pandit-417a4521b>

<panditsamik25@gmail.com>
