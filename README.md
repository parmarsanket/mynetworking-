
# Networking Factory Examples (Retrofit, Ktor, Volley)

This document provides examples for creating networking factories for **Retrofit**, **Ktor**, and **Volley** in Kotlin.

---

## 🚀 Retrofit Factory

```kotlin
object RetrofitFactory {
    @OptIn(ExperimentalSerializationApi::class)
    fun createRetrofitInstance(): Retrofit {
        val json = Json {
            ignoreUnknownKeys = true
            prettyPrint = true
            isLenient = true
        }
        return Retrofit.Builder()
            .baseUrl("https://ddragon.leagueoflegends.com/cdn/15.7.1/data/en_US/")
            .addConverterFactory(json.asConverterFactory("application/json".toMediaType()))
            .build()
    }
}
```

### Explanation:
- **Retrofit**: Handles API requests with a `ConverterFactory` to serialize and deserialize JSON.

---

## 🚀 Ktor Factory

```kotlin
object KtorFactory {
    fun createKtorClient(): HttpClient {
        return HttpClient(CIO) {
            install(ContentNegotiation) {
                json(Json {
                    ignoreUnknownKeys = true
                    prettyPrint = true
                })
            }
            defaultRequest {
                url("https://ddragon.leagueoflegends.com/cdn/15.7.1/data/en_US/")
            }
        }
    }
}
```

### Explanation:
- **Ktor**: Uses a `HttpClient` to send requests, with `ContentNegotiation` for JSON support.

---

## 🚀 Volley Factory

```kotlin
object VolleyFactory {
    fun createVolleyRequestQueue(context: Context): RequestQueue {
        return Volley.newRequestQueue(context).apply {
            start()
        }
    }

    fun createStringRequest(url: String, responseListener: Response.Listener<String>, errorListener: Response.ErrorListener): StringRequest {
        return StringRequest(Request.Method.GET, url, responseListener, errorListener)
    }
}
```

### Explanation:
- **Volley**: Uses `RequestQueue` and `StringRequest` to handle network requests with a listener.

---

##retrofit
```
interface ProductApi {

    @GET("products")
    suspend fun getProducts(): List<Product>

}

object RetrofitInstance {

    private const val BASE_URL = "https://fakestoreapi.com/"

    private val json = Json {
        ignoreUnknownKeys = true
        prettyPrint = true
        isLenient = true
    }

    private val logging = HttpLoggingInterceptor().apply {
        level = HttpLoggingInterceptor.Level.BODY
    }

    private val client = OkHttpClient.Builder()
        .addInterceptor(logging)
        .build()

    val api: ProductApi = Retrofit.Builder()
        .baseUrl(BASE_URL)
        .client(client)
        .addConverterFactory(
            json.asConverterFactory(
                "application/json".toMediaType()
            )
        )
        .build()
        .create(ProductApi::class.java)

}
```
