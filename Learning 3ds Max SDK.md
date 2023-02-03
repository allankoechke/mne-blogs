![image001](https://raw.githubusercontent.com/allankkoech/mne-blogs/master/img/blog2/image001.png)  

*Image adapted from [http://aneeswork.blogspot.com/2015/10/3d-interior-design-3ds-max-vray.html](http://aneeswork.blogspot.com/2015/10/3d-interior-design-3ds-max-vray.html)*
<br />

# Learning 3ds Max SDK: My Journey
One thing has always fascinated me for a very long time, that is, programming. I used to really wonder how big software did work and how they came to being. The absolute beauty of just writing English grammar with some semantics and later the computer gets to understand it, isn’t that just awesome?  
  
My journey learning 3ds Max and 3ds Max SDK was not that easy. Let’s be frank, transitioning into a new codebase, semantics, UI, documentation, etc. is overwhelming at start.  

## So, why learn the SDK?
Let’s address the elephant in the room, the why question. It’s with no doubt if you ever used any software, you ever felt like, ***“I wish I had this feature”*** or ***“I wish this was simpler”*** or “I wish I could be able to just automate these tasks”. Yes, I know the frustration of sometimes finding out what you wanted isn’t available or is just long and tedious. We mostly solve some of such tasks with keyboard key-bindings (shortcuts) but some tasks are not that simple for such operations. That’s where the SDK comes into play. Giving you the ability to make and tweak as you wish.  

With access to the APIs through the SDK, you are given the power to make your own custom plugins to add or modify existing tasks, automate operations like rendering, generations, etc. and much more! But with such power comes some responsibility, let’s look at them in a bit!  
  
## Prerequisites
To be able to learn 3ds Max and its SDK, you require some tools and knowledge. 
- **Programming knowledge** – Definitely this is a must. Though not a mastery of the subject but a good understanding of the basics. Of much concern here is C++ although the SDK also has .NET and Python bindings. In C++, these are some topics that you will require understanding to work well with the SDK: classes, objects, inheritance, polymorphism, virtual functions, abstract classes, pointers, references, etc.
- **A good computer/laptop** – Check https://knowledge.autodesk.com/support/3ds-max/learn-explore/caas/sfdcarticles/sfdcarticles/System-requirements-for-Autodesk-3ds-Max-products.html for a detailed machine requirements to be able to run 3ds Max smoothly. Depending on the version of 3ds Max you intend to run, the machine specification may be a bit different. 
- **Visual Studio IDE** – Though not a compulsory requirement, for a newbie it may be. The IDE will handle most setup, environment variables etc. automatically if using the plugin wizard. Programming can still be done using a normal code editor (i.e., Visual Studio Code or Notepad) and build & compile done on the Command Line (CMD). Visual Studio IDE Community, Professional or Enterprise will work for this setup.
- **3ds Max & 3ds Max SDK** – Installation of 3ds Max and its SDK is required. Check out how to install and setup the SDK here https://knowledge.autodesk.com/support/3ds-max/troubleshooting/caas/sfdcarticles/sfdcarticles/How-to-install-the-Software-Development-Kit-SDK-for-3ds-Max.html. Also do setup the plugin wizard for easy project setup on visual studio IDE by following the instructions here https://help.autodesk.com/view/MAXDEV/2023/ENU/?guid=installing_the_plug-in_wizard_ap.
- **Qt Knowledge** – *[OPTIONAL]* If you ever need to build plugins that will have user interactions, 3ds Max has support for adding UI based on Qt Widgets as from 3ds Max 2017. Check https://help.autodesk.com/view/MAXDEV/2023/ENU/?guid=using_qt_with_3ds_max_plug-ins for instructions of setting up Qt with Visual Studio.
- ***Patience*** – Learning takes time, takes practice, takes iterations. Give yourself time to internalize the concepts. No one becomes a pro overnight. 
  
<br />
<br />

*NOTE: This is very important to note: Depending on the 3ds Max version you chose, let’s say 3ds Max 3ds Max 2023, each version supports a specific SDK version, Visual Studio compilers, Qt version, etc. Always check for the right versions from the site https://help.autodesk.com/view/MAXDEV/2023/ENU/?guid=sdk_requirements. This gives each MAX version and the compatible OS, SDK, C++ compiler, .NET framework and Qt version.*  

<br />  

## So where do I start?
With all the pre-requisites satisfied, am sure you have a strong quest to get your feet on the move. 3ds Max is a big software, developed for decades. As such, it comes with history of then and now. Depending on the material you can get your hands on to read or watch, you may be getting old concepts that may have been deprecated or changed to new formats. Always keep that in mind.  
<br />

3ds Max introduces some new terminologies, youll get to hear things like modifiers, reference systems, parameter blocks, nodes, etc. Delving into these topics right away seems a bit overwhelming. Here is how I handled it:

- ***Learn 3ds Max UI basics first*** – To understand how things work behind the scenes, we need to know how its handled on the scene. User interaction with the software is important and most critical and a s such, knowing how to work as a user gives you knowledge of how objects are modified stack by stack till you attain a desired 3d object. The areas to start with are:
  * **Basic geometric objects** – Getting to know how to create box, sphere, cylinder, etc. objects on scene. This also entails, how to modify the parameters of such objects like length, width, height, segments, etc. 
  * **Transforms** – This is basically 3d manipulations on an object on scene. It’s applied on the entire object and its independent of the shape of the scene object. It’s the object position, rotation, and scale. 
  * **Modifiers** – This is how the basic shapes are changed. Adding modifiers on them allows manipulation of the underlying object. For instance, adding a bend modifier on a straight object gives an output as shown below. Check out how such a modifier is added and resulting effect on the object here https://help.autodesk.com/videos/0zd2FvbTrAtMzQjiHcH39SnEZstxFDYC/video.webm. Other types of modifiers are: Edit poly, OpenSubdiv, chamfer, shell, etc.     
  <br />
  <br />  

  ![image002](https://raw.githubusercontent.com/allankkoech/mne-blogs/master/img/blog2/image002.jpg)  

  <br />
  <br />  

  * **Animation and animation controllers** – This entail adding motion to scene objects. 3ds Max comes with a couple of animation controllers that you can attach to your object on scene. The keyframe timeline allow for keyframe manipulation of the animation motion. A basic understanding of how to do animation and use the different tools on the UI for the same purpose is important to working with animation on the SDK later. Learn more here https://help.autodesk.com/view/3DSMAX/2019/ENU/?guid=__interactive_help_animation_animation_controllers_html.
  * **Lighting and Rendering** – After all modification on objects has been completed, we add lights to give a realistic object before it rendered out to a desired output format. This section will give insights of the different render engines and how they work, how to work with different light types, working with cameras, etc. Here is a pictorial of scene lighting. Read more on this here https://help.autodesk.com/view/3DSMAX/2019/ENU/?guid=__interactive_help_lighting_rendering_html.  
  <br />
  <br />  

  ![image003](https://raw.githubusercontent.com/allankkoech/mne-blogs/master/img/blog2/image003.jpg)  

  <br />
  <br />  

  * **Materials** – Creating realistic objects require a look a feel with what we do interact with in our day to day lives. Materials are data that is assigned to the surface/faces of objects to make it appear a certain way when rendered. They include but not limited to iron, steel, leather, wool, etc.  

<br />

- ***Learning the SDK*** – With the UI basics covered, learning the SDK is less overwhelming. So, how do you go about it?
  * **Hello World!** – As any standard practice, begin with a hello world program. 3ds Max SDK documentation has a good tutorial to hep you do exactly this. Check out https://help.autodesk.com/view/MAXDEV/2023/ENU/?guid=lesson_1_sample_utility_plug-in on how to begin a simple utility plugin. *[I do assume you did setup the plugin wizard on your visual studio installation]*. The code sample below shows a text Hello World on the prompt stack in the 3ds Max UI.  
  <br />
    ```js  
    ip->PushPrompt(_M("Hello World from Lesson 1"));  
    ```  
    <br />

    With that out of the way and well understood, lets now look at other concepts of the SDK.  

  * 3ds Max SDK: The Learning Path – This is a series of lessons covering different aspects of the SDK. The tutorial is available at https://help.autodesk.com/view/MAXDEV/2023/ENU/?guid=3ds_max_sdk_-_the_learning_path. Let’s have a rundown of what it entails:
    * ***Sample Utility*** – This is the hello world program for a utility plugin. Will give you a rundown of what it takes to setup a plugin project, explanation of all files in the project as well as what the different functions generated by the plug-in wizard generated.
    * ***Scene Graphs & Nodes*** – Scene objects (geometric objects, lights, cameras, etc.) in 3ds Max and the relationship with each other are stored in a scene graph. This gives a good understanding of the scene graph structure, how to transverse the scene graph and manipulate the nodes (add, remove, update as well as a sample code that shows a simple graph exporter into a text file.
    * ***Reference System*** – Think of this to get updates from changes when entity changes in scene. From a Qt background, think of it has the signal/slot mechanism but way different. This gives an understanding of how plugins can propagate changes on scene through which other plugins/objects can get the changes. A good understanding of this subject will make it easy developing plugins since most times plugins depend on external data on scene and need to be notified of the changes as well as modify them and cause appropriate change propagation.
    * ***Animation Controllers*** – These are plugins that provide 3ds Max the value of a parameter for a scene object at any given time. Think of it as a function that given the time (keyframe timestamp) it returns the property value at the given time. This section goes through how to implement a simple controller to achieve scene object animation over time.
    * ***Geometric Objects*** – On the 3ds Max scene, objects created by mouse clicks, or any other means is because of combination of one or more geometric shapes. This is used for creating 2D shapes like circles, squares, boxes, triangles, etc. or even 3d like cylinder, box, etc. God understanding of this gives enough knowledge to aid in developing custom shapes and objects on scene or even modify existing shapes. 
    * ***Parameter Blocks*** – This is a 3ds Max way of reading from and writing to plug-in parameter values which is independent of the plug-in type. This is 3ds Max specific implementation giving ease of sharing plugin data, easy updates by other plugins, easy serialization, etc. This section seems intimidating at first to grasp but it’s one of the most critical to understand well for you to develop plugins that can maintain and share parameter states with other plugins.   
<br />
<br />

- **3ds Max Webcast Tutorials** – Similar to the learning path above, the webcast was initially done as virtual tutorials to train on 3ds max SDK topics with assignments after each topic. Unlike the learning path, the webcast goes an in-depth with explanations of why thigs are done the way they are done. This gives a better understanding of the concepts conveyed both in the documentations and the learning path tutorials. With an addition of assignments after each section, this gives room to try out from knowledge learnt with assurance of explanation of the required response if not well understood or if stuck. Although this content is old, upgrade of such content is underway. Let’s look at the main sections that prove to be useful for a beginner.
  * ***General 3ds Max customization Introduction*** – This goes through 3ds Max UI overview and how it can be customized, a rundown of MAXScript usage in scene objects manipulation, the .NET API in 3ds Max and 3ds Max SDK usage.
  * Getting started in the 3ds Max SDK basics – This section goes into the SDK basics, terminologies, plug-in types, and a rundown of the basic scene concepts (Scene, Node, References, Parameter Blocks, Animation, etc.)
  * ***Transformation and Scene, Node, Scene Graph*** – Scene objects are represented by Nodes on the scene graph. So, this section explains how transformation (translation, rotation, scale) of the scene is done, scene compositions, scene objects and scene graphs manipulation & its traversing.
  * ***Reference System*** – If you didn’t understand how reference systems work from the Learning path, this will be your teacher. It goes in-depth on how reference systems work, the logic behind, as well as good/bad practices in reference systems in 3ds Max.
  * ***Parameter Blocks and Function Publishing*** – Similar to what is detailed in the learning path, this explains parameter blocks systems, its implementation and expound on the concepts that make it such a powerful concept behind the 3ds Max robustness. In addition, this section goes to give an introduction into function publishing - a concept for exposing functions in C++ to the MAXScript.
  * ***Geometry Fundamentals*** – Further from what was covered by the geometry section of the learning paths, this section goes further into covering shapes, splines, patches, and meshes in the making & manipulation of scene objects.
  * ***Modifiers*** – Similar to what was mentioned under 3s Max UI learning, modifiers here look at how to make custom modifiers that manipulate objects, edits, mapping coordinates and world space. Such modifiers are bend, taper, twist, extrude, surface, etc.   

<br />

# Tips & Tricks
Fast learning does exist, but pitfalls are the hindering factor. Here are my suggestions based on what I did learn in my journey:
- *Use the Webcast & the Learning path tandem* – Some of us like reading, some like watching, depending who you are, try watching the concepts and listening to the in-depth explanations then later go do the reading on the learning path and/or the 3ds Max SDK documentation. This may save you hours of wondering what you are reading.
- *Don’t go too deep* – Quite controversial, but, if your intent is to work with the SDK then, take care you don’t get too deep learning the UI. Autodesk gives recommendation of tutorials to follow to learn the UI section, which is very detailed. You may look at specific sections to know the basics then later go deeper as needed.
- *C++ is important!* – Learning the SDK requires some level of understanding C++ concepts. Sharpen your knowledge/skills there first since there is less/no explanations of such concepts once you start working with the SDK.
- *Be careful with burnout* – Yes, this is real. The SDK concepts are a lot, I don’t expect you to master all in a single day. Give yourself time, rest whenever you feel overwhelmed, and most important – reach out for any questions.
- *Make SDK examples & samples your charm!* – The 3ds Max SDK ships with samples and example code which you can refer to see how things were implemented. Be careful to always change the plug-in class IDs to avoid conflicts with the default implemented plug-ins. 
- *Check attached samples for tutorials* – Both the Learning path and webcast material have completed code samples. Check on them in case things are not working as expected. The Learning path and the webcast Github/videos/materials can be found in the links below.

---

Learning Path Github Repository - [https://github.com/ADN-DevTech/3dsMax-Learning-Path](https://github.com/ADN-DevTech/3dsMax-Learning-Path)  
3ds Max Developer Center - [https://www.autodesk.com/developer-network/platform-technologies/3ds-max#3dsmax-sdk](https://www.autodesk.com/developer-network/platform-technologies/3ds-max#3dsmax-sdk)  

<br />  

> Developer center provides 3ds Max SDK Training Webcasts in both [English](http://download.autodesk.com/media/adn/3dsMax_July2012_Webcast_final_Recording.zip) and [Japanese](https://images.autodesk.com/adsk/files/3dsMax_SDK_webcast_Japanese.zip) and a separate [Download](http://download.autodesk.com/media/adn/3dsMax_July2012_Webcast_final_TrainingMaterial.zip) for the training materials. Videos are provided in a zip file.
---
<br />

***I hope this will help you in your journey to learning 3ds Max SDK.***   

<br />

Any question? Any suggestion? Feel free to reach out!

<br />
<br />
