# ANDROID ROAD MAP:

1. What is and how to use Gradle:  
   1.1 [A Beginner's Guide to Gradle](https://medium.com/@andrewMacmurray/a-beginners-guide-to-gradle-26212ddcafa8)  
   1.2 [Gradle Tutorial Video](https://www.youtube.com/watch?v=cUGWEQ8NLHk)  
      1.2.1 When we develop an app for the user, it must be tested, packaged, and published (build app). This can be simplified with Gradle, which is a build automation tool.  

2. Activity - Single Screen / Fragment - portion of activity:  
    2.1. Lifecycle:  
        2.1.1. onCreate: Called when it's first created. Set up the activity here (from layouts).  
        2.1.2. onStart: Called when it's visible to the user. Perform animation and UI updates here.  
        2.1.3. onResume: Called whenever the activity is in the foreground.  
        2.1.4. onPause: Called when the activity is no longer in the foreground. Save any important data here.  
        2.1.5. onStop: Called when it's not visible to the user. Release resources.  
        2.1.6. onDestroy: Called when the activity is destroyed.  

    2.2 State Changes:  
        2.2.1. Configuration changes:  
            a. Whenever orientation changes in the app, the original activity will be destroyed, and a new instance of the activity will be created.  
            b. [onPause -> onStop -> onSaveInstanceState -> onDestroy = original]  
            c. [onCreate -> onStart -> onRestoreInstanceState -> onResume = new]  
            d. While restoring, views with ID will preserve their state. If you want to save additional data, use onSaveInstanceState, Shared Preferences, View Model, Room DB.  

        2.2.2. User Taps Back Button:   
            When the user is pressing the back button, they are not intended to go back to that activity. So in that case, onSaveInstanceState won't be called.  

        2.2.3. Activity/Dialog partially covers the current activity. onPause and onResume.  
        2.2.4. Activity/Dialog completely covers the current activity. onPause -> onStop and onRestart -> onStart -> onResume.
        2.2.5. System kills the process  
        2.2.6. [Task_&_BackStack](https://www.youtube.com/watch?v=Z0AzoFOiH9c)  
            a. Assume the browser app is opened on a phone. The browser home screen will be pushed to a stack called the back stack. When clicking on a bookmark, the bookmark screen will be pushed to the back stack and will be visible to the user by moving the previous screen to the background. When clicking on the back button, the current screen will be popped, and the previous screen comes to the foreground.  
            b. Grouping similar screens/activities is called a task. E.g., Browser task (home, bookmark, and search) Instagram task (home, reels, profile).  
            c. Launch modes: Tell what should be done when a new activity is pushed onto the back stack. 
            d. Standard: Lets say Chrome and Instagram are currently opened, and by clicking a URL in Instagram pushes a new browser activity.  
            e. Single Top: If there is any active browser activity while clicking on a URL, it will use it instead of creating new.  
            f. Single Task: Similar to single task, only difference is if suppose the browser activity is available along with other activities of same task, it make sure creates a new task and pushed the new broswer activity such that when user taps on back button, it will navigate to previous screen rather than being in the previous task pushed in the same task  
            [Single Task](single_task.png)  
            g. Single Instance: Similar to Single Task, the difference is, this is isolated from the other task and not allows to call any other activity within the same task. Eg. Payment gateway where it wont allow to create new activity of the browser  

3. [Services_&_Intent_Services](https://medium.com/@fierydinesh/understanding-service-and-intentservice-in-android-with-kotlin-cea76512ec16)  
    a. Go for Intent Service whenever wants to perform any long running task in the background even when app is not in the active state. eg. downloading file. It always runs in new thread. [Explicit_Termination] [No_MULTI_THREADING]  
    b. While in Service, it will run in main thread. So while performing long running task, make sure to create new thread. eg. music playback, downloading file [Implicit_Termination] [MULTI_THREADING]  
    c. While Service is suitable for long-running tasks that need to be explicitly stopped, IntentService is useful for executing short-lived background operations that automatically stop once their work is done.  

4. [Broadcast & Broadcase Receiver](https://www.youtube.com/watch?v=HDVyFsFUuVg)  
    a. In order to communicate between apps and the system we use this.   
    b. We use broadcase to send data to other apps[not often]  
    c. We use broadcase receiver to receive data either from other app[not often] or system[often]. Example, checking user toggles internet connection, bluetooth, airplane mode etc..  

5. [Content_Provider](https://www.youtube.com/watch?v=IVHZpTyVOxU)  
    a. When we use any app, it is going to store some data which only be accessed within the app. Other apps wont access but with content providers we can get access to other apps data and we can give access to our app's data through content uri. Example: Loading all images, contact list etc.. from other apps.

6. [Intent_&_Intent_Filters](https://www.youtube.com/watch?v=2hIY1xuImuQ)    
    a. Intent: request an action from other app components[activity, service]  
    b. Explicit Intent: Here we explicitly mention which component should be launched
    c. Implicit Intent: Here we mention the what we want [eg want to share a image], and android s/m takes care of finding the apps which can open image with. It will give us the chooser where we can choose a app to share image
    d. Intent filter: Let's say we are chrome. We searched an image and want to share it. By clicking on it, android popups the chooser, where it'll all the apps which can load image. If we want to register our app also to accept image, we can go for intent filter.

    Example: Lets say pdf viewer app. When it is opened, it will load all the pdf files in the app with the help of content provider. Lets say we download a pdf file in whatsapp. While opening, it will ask for app to open in whatsapp. To make sure the pdf viewer is shown in the chooser, we can register the app in the intent-filter[Intent.ACTION_SEND]
