# README #
## Installation

1. Download FormBuilderSDK.arr and its build.gradle file.
2. Change the project structure of your app from Android to Project.
3. Create a new folder in the same directory where your 'app' module is located and name it FormBuilderSDK (same as the .aar).
4. Paste the downloaded files there.
4. In the project level settings.gradle file, add include(":FormBuilderSDK") under include(":app") or include(":app", ":FormBuilderSDK")
5. In the app module build.gradle, add the following line in the dependencies section implementation(project(":FormBuilderSDK"))
6. Check the app module build.gradle (from step 5) if you have the following dependencies in your project:\
   implementation( "io.insert-koin:koin-android:3.5.6")\
   implementation( "com.squareup.retrofit2:converter-gson:2.6.4")\
   implementation( "com.squareup.okhttp3:logging-interceptor:4.9.2")\
   If not please add them.
7. Sync the project and do a clean rebuild
8. Now you can proceed to the "How to use ?" section


## How to use ?

```kotlin
val intent = Intent(this, StartFormBuilderActivity::class.java)
intent.putExtra("processID", "PUT_PROCESS_ID_HERE")
intent.putExtra("callingServiceToken", "PUT_SERVICE_TOKEN_HERE")
startActivity(intent)
```
This creates an instance of FormBuilder and then opens a new interface based on the type of the process corresponding to the provided PROCESS_ID.
The callingServiceToken argument is a unique client identifier that will be stored in the submission. Calling service token can be null.

```kotlin
intent.putExtra("stepID", "PUT_STEP_ID_HERE")
```
This creates an instance of FormBuilder and then opens a new interface of a previously created process at the step which the user has reached before (e.g. if they have interrupted it for some reason).
It can also just be the first step for a process that the backend has preconfigured in some way. Either put processID or stepID when creating the intent.

```kotlin
intent.putExtra("baseURL", "PUT_BASE_URL_HERE")
```
You can put your own base url, to which you can send your requests, or you can simply not add it and use the default url.

```kotlin
FBDocDataFragment.ProcessResult.getProcessResultCallback { success, error ->
   if (success)
   //Do something on success
   else
   // Handle error
}
```

Calling getProcessResultCallback tells you if the started process finished successfully or if there was an error with the process

Optionally you can pass an FBAppearance object in the intent.putExtra when starting the FormBuilder, with which you can configure some of the appearance of the labels, fields, buttons:

```kotlin
val appearance = FBAppearance(
   textColor = ContextCompat.getColor(this, R.color.purple_500),
   backgroundColor = ContextCompat.getColor(this, R.color.teal_700),
   buttonTextColor = ContextCompat.getColor(this, R.color.color_blue),
   buttonBackgroundColor = ContextCompat.getColor(this, R.color.myColor),
   progressBarColor = ContextCompat.getColor(this, R.color.teal_700),
   viewTextColor = ContextCompat.getColor(this, R.color.purple_500),
   viewBackgroundColor = ContextCompat.getColor(this, R.color.white)
)

val intent = Intent(this, StartFormBuilderActivity()::class.java)
intent.putExtra("processID", "PUT_PROCESS_ID_HERE")
intent.putExtra("appearance", appearance)
startActivity(intent)
```
If you don’t pass an appearance object, the SDK will use default values.
