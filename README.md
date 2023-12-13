## Setting up Koin and Ktor —
 Add both dependency to the :shared module, I’ve used gradlelibrary catalog for dependencies.
    io.insert-koin:koin-core:3.5.0
For Ktor, there are multiple dependencies each with their own purpose depending on your API.

Since, Injection will be done at platform level so we need to expose the initialization part to both platforms. To do this we’ll create a Helper class with a method initKoin() and call this method from application classes for iOS and Android.

initKoin() → We’ll define all our dependencies here which we’ll be needing later on.


// :shared/src/commonMain/kotlin/di/Koin.kt

fun initKoin() = startKoin {
    modules(networkModule)
}

private val networkModule = module {
    single {
        HttpClient {
            defaultRequest {
                url.takeFrom(URLBuilder().takeFrom("https://internshala.com/"))
            }
            install(HttpTimeout) {
                requestTimeoutMillis = 15_000
            }
            install(ContentNegotiation) {
                json(Json {
                    ignoreUnknownKeys = true
                    prettyPrint = true
                })
            }
            install(Logging) {
                level = LogLevel.ALL
                logger = object : Logger {
                    override fun log(message: String) {
                        println(message)
                    }
                }
            }
        }
    }
}


![Screenshot_2023-12-13-19-01-05-058_com learning app android](https://github.com/mkathiravan/KMP_LearningApp_Pagination/assets/39657409/be6d9695-69d6-422b-9c92-d46b4403aace)
