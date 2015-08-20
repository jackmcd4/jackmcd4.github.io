---
layout: post
title: Deploying with Microsoft's Azure
---

  Deployment isn't fun. No one enjoys it, but there's no way around it.
I spent two days this past week deploying my first app through Microsoft's Azure.
Their site is hard to navigate, laggy, and has long load times that would make anyone nervous,
even more so when your baby is on the line (and by baby I mean fresh new app that you've pawed 
over for countless hours). A great way to think about this is that azure is merely taking all 
that time to process you baby and handle it with the utmost care.

  Here's a few general tips to keep in mind about pre-deployment:

* It is hard to debug during deployment, so write tests and run them before!
 Making sure your app is ready for deployment is just as important as deploying it. 
 Who cares if you're great at deployment when everything you send out is buggy?

* On a similar note, check for bugs as often as you can.
 As frustrating as this might sound it can save a lot of effort in the future. 
Hunting through a mass of code only to realize hours later that an apostrophe 
was accidentally outside a script tag is no fun.

* One further digression...
Never...ever...write a file in plain text (besides a README).
Sublime text - or wherever you write your code - (hopefully)
implements a nice color scheme that will prevent such trivial
errors as mentioned above from occuring.

* Grunt makes everything easier.
There are loads of plugins available to help make your app run smoother, help
with deployment, and cut down on its size. Gulp is a similar equivalent and use 
comes down to personal preference.

  Grunt's getting-started tutorial:
     
  <http://gruntjs.com/installing-grunt>

  It is pretty easy to get started with grunt if you don't want to read through it... 

  Four options:
  
```javascript
npm install -g grunt //globally install grunt
npm install -g grunt-cli //globally install grunt command line interface (CLI) version
npm install grunt --save-dev //Save to local repository
npm install grunt-cli --save-dev //Save to local repository, command line
```
 
  Their website also has a search bar built-in for plugins.
  Check out concat, css-min, uglify, and the many others available:

  <http://gruntjs.com/plugins>

  Choose a database wisely. 
  If you end up needing to change over from one database to another, while common,
  it can be tricky and refactoring takes time. Use as much foresight as possible and, 
  as always, research!

  Two really common databases are mongoDB and mySQL. The former is less opinionated, while the second
  is seriously structured. There is much more that goes into this, so check out these:

  Basics:
    
  <https://www.mongodb.org/>
    
  <https://www.mysql.com/>
        
  Helpers:
    
  <http://www.sqlite.org/>
    
  <http://docs.sequelizejs.com/en/latest/>
    
  <http://mongoosejs.com/>

If you do decide to go with Microsoft Azure, you can quite easily set up your app for deployment 
through the command line with Azure's CLI. Follow these steps to get your account set up and
your app ready to go in no time!
    
  1. If you don't have one already, make a Microsoft account. A forewarning about this, if
  you are running on the same IP address as others who have made an account that day you will
  be told you have made too many accounts that day. Simply sign into another wifi hotspot or
  create an account on a smartphone using cellular data. 

  2. Go to the Azure website and sign up for a 30-day free trial. Even if you're ready to start
  deploying I'd reccommend not diving into a full subscription before you try it out. At the end
  of the day there are other places to deploy from, such as Heroku or Amazon Web Services. 

  3. Open terminal and run the following command which will install azure's CLI and allow you
  to easily deploy straight from terminal:
```javascript
npm install -g azure-cli
```
  4. Free account or not, you need to download an authentication string that azure will open
  in your browser...The following commands will set up your account
```javascript
azure account download //opens a new page in your browser and downloads file
azure account import <file> //once you have typed the first three words you can simply drag the file
//into the command line
azure account list //you should see something like this:

info:    Executing command account list
data:    Name        Id                                    Tenant Id  Current
data:    ----------  ------------------------------------  ---------  -------
data:    Free Trial  <SHA CODE GIVEN BY AZURE HERE>        undefined  true   
info:    account list command OK

azure account set <SHA> //paste your SHA from the Id column above (Secure Hash Algorithm)
```
  5. Now that your account is set up, you can create a sitename where all the info from your app will
  be stored. 
```javascript
//if you haven't been inside your repository, make sure you go there before the next line of code

azure site create <NAME OF APP/WEBSITE> --git
/*
  This sets up a remote for you repository under the name 'azure'
  This means you can magically add information to your application by writing 'git push azure master'!
  You can set this up in your grunt file so that anytime you deploy it automaticly runs that line
  of code in the command line
*/
```
  6. Set up environments for your app to be able to use on deployment such as node, mongolab, etc.
```javascript      
azure site appsetting add <key>=<value>
```  
  7. A note on above, if you are using a database helper like mongolab, make sure this is where you save 
  the SHA they (mongolab) give you, NOT in your source code. It is a huge security risk for someone to 
  have access to that code and thus your database. 
```javascript
//Src-Code
mongoURI = process.env.MONGOLAB_URI || 'mongodb://localhost/myAppName';

//Terminal
azure site appsetting add MONGOLAB_URI=<MONGO_LAB SHA>
//Azure ensures the safety of that hash
```
  8. Lastly decide how you want to scale your app (which can be costly), azure has a great article about
  this that you can find below:

  <https://azure.microsoft.com/en-us/documentation/articles/web-sites-scale/>
  
  9. Once you've figured out your scale run these lines of code, and you're deployed!
```javascript
azure site scale mode <MODE-TYPE> <SITE-NAME>

//deploy with either grunt:
grunt deploy --prod 
//or simply:
git push azure master
```
  10. Now that you're deployed, of course you want to see what it looks like...

  Type this line:
```javascript
azure site browse 
```
  Or go to: http://www.YOURSITENAME.azure.microsoft.com



