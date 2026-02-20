# README #
## Installation

1. Download FormBuilderSDK.arr and its build.gradle file.
2. Change the project structure of your app from Android to Project.
3. Create a new folder in the same directory where your 'app' module is located and name it FormBuilderSDK (same as the .aar).
4. Paste the downloaded files there.
4. In the project level settings.gradle file, add include(":FormBuilderSDK") under include(":app") or include(":app", ":FormBuilderSDK")
5. In the app module build.gradle, add the following line in the dependencies section implementation(project(":FormBuilderSDK"))
6. Check the app module build.gradle (from step 5) if you have the following dependencies in your project:\
   implementation( "androidx.core:core-ktx:1.13.1")\
   implementation( "io.insert-koin:koin-android:3.5.6")\
   implementation( "com.squareup.retrofit2:converter-gson:2.9.0")\
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
intent.putExtra("showCancelButton", true)
```
Optionally you can show a cancel session button while streaming. Default is set to false

```kotlin
FBDocDataFragment.ProcessResult.getProcessResultCallback { success ->
   if (success)
   //Do something on success
   else
   //Do something on failed
}
```

Calling getProcessResultCallback tells you if the started process finished successfully or if there was an error with the process

```kotlin
 FormFragment.ProcessResult.postToHostUI { status ->
    // Parameter status - the status from the SDK
}
```

postToHostUI is called when a field with a specific type is received

Optionally, you can pass an FBAppearance object in the intent.putExtra when starting the FormBuilder SDK, with which you can configure the appearance of the labels, fields, buttons:

```kotlin
val appearance = FBAppearance(
   backgroundColor = ContextCompat.getColor(this, R.color.teal_700),            // Background of all screens in the SDK

   toastFont = R.font.momoregular,                                              // Font of the directions text during KYC 
   toastTextSize = 16,                                                          // Text size of the directions text during KYC
   toastTextColor = ContextCompat.getColor(this, R.color.white),                // Text color of the directions text during KYC
   
   textFont = R.font.momoregular,                                               // Font of the text that is displayed only (not editable text)    
   viewTextSize = 16,                                                           // Text size of the text that is displayed only (not editable text)
   viewTextColor = ContextCompat.getColor(this, R.color.purple_500),            // Color of the text that is displayed only (not editable text)
   viewBackgroundColor = ContextCompat.getColor(this, R.color.white),           // Color of the background of displayed text

   inputFont = R.font.momoregular,                                              // Font of the input text
   inputTextSize = 16,                                                          // Size of the input text            
   inputTextColor = ContextCompat.getColor(this, R.color.white),                // Color of input text field
   inputBackgroundColor = ContextCompat.getColor(this, R.color.purple_500),     // Background color of editable text field
   
   buttonFont = R.font.momoBold,                                                // Font of the button text
   buttonTextSize = 16,                                                         // Text size of the button text
   buttonCornerRadius = 30f,                                                    // Corner radius of buttons (0f for square button, anything above 0f makes corners rounded)
   buttonTextColor = ContextCompat.getColor(this, R.color.color_blue),          // Color of the button text
   buttonBackgroundColor = ContextCompat.getColor(this, R.color.myColor),       // Background color of buttons
   
   progressBarColor = ContextCompat.getColor(this, R.color.teal_700),           // Color of the circular progress bar that appears when changing between screens or loading requests
   
   linearProgressIndicatorColor = ContextCompat.getColor(this, R.color.white),  // Color of the indicator that is loading while scanning your ID in KYC
   linearProgressTrackColor = ContextCompat.getColor(this, R.color.myColor)     // Color of the track that is shown while scanning your ID in KYC
)

val intent = Intent(this, StartFormBuilderActivity()::class.java)
intent.putExtra("processID", "PUT_PROCESS_ID_HERE")
intent.putExtra("appearance", appearance)
startActivity(intent)
```
If you don’t pass an appearance object or a certain property, the SDK will use default values.