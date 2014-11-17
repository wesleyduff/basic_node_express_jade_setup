## GET STARTED

Find a tutorial on this on my [portfolio site](http://www.wesduff.com/node-express-jade-tutorial/)

    $ git clone https://github.com/wesleyduff/basic_node_express_jade_setup.git

    $ npm install

Start the server on port 3000

    $ node server

Basic light version of a basic Node Express Jade install. 
This is a good base to start off of to include any other features you may like to add.

# TUTORIAL

In this tutorial I will cover the steps needed to get started with Node.js and Express.js from the ground up. Express does come with a generator but I do not go into generated Express apps. This tutorial is a bare bones example of getting Node, Express and Jade to work together to display a home page.

## Get Started

I will be writing this tutorial to be used in Windows 8.1. Make changes to the command line as needed for your OS

    $ mkdir nodejs_starter
    $ cd nodejs_starter
    

Check to see what version of *Node* you have.

    $ node -v
    

If the printed response says something like "node is not recognized as an internal or external command." then you will need to install nodejs. You can download Nodejs [here][1]

Now we should be ready to get started with our Node.jS setup.

To start off you will need a package.json file. This file will tell node where to get all of your application dependencies, who wrote the application, what version it is on and a lot more. Lets start off by using **NPM** (node package manager) to create our package.json file.

*   Create our package.json file using npm.
    
    $ npm init

*   Inside the command line after you run *npm-init* you will be asked some questions.
    
    *   name : We will call our name **Node JS Starter App** * version : Start out our vision at **0\.0.1** 
    *   description : Lets add a description of **Node JS Starter App - The beginning** 
    *   entry point : Add an enty point for our node.js application with this file **server.js** 
    *   test command : Just press enter. We will add this later. 
    *   git respository : Press enter again. We will add this later. 
    *   keyowords : Lets add some keywords. It is always good practice. **Node, Express** 
    *   author : Add your name as the Author 
    *   License : We will use the [MIT license][2] **MIT** 
    *   You will see a JavaScript object printed out in the command window and a question below reading *is this ok?*. 
    *   press enter

Your result should look similar to:

    {
      "name": "nodejs_starter",
      "version": "0.0.1",
      "description": "Node JS Starter - It beginns",
      "main": "server.js",
      "scripts": {
          "test": "echo \"Error: no test specified\" && exit 1"
      },
      "keywords": [
          "Node",
          "Express"
      ],
      "author": "Your name",
      "license": "MIT"
    }
    

Next we need to install express.js and add it to our project.json file. We can do this all at once in our command window using this command.

    $ npm install express --save 
    

The *--save* tells npm to update our package.json file with the express dependency. This also installs the node module to our folder. You can see this folder has been created inside your *nodejs_starter* folder.

**note** you can also run `npm install` to install all the node modules that are dependencies listed inside your package.json file.

This post will continue with the use of Express 4.0. With the new version of Express we will need to install its required dependencies. These dependencies include

*   serve-favicon
*   morgan 
*   method-override 
*   express-session
*   body-parser
*   multer errorhandler
*   jade

* * *

Lets install these dependencies into our app using the command line.

    $ npm install serve-favicon morgan method-override express-session body-parser multer errorhandler jade@latest --save
    

Our package.json file will get updated and should now have the dependencies shown below

    {
      "name": "nodejs_starter",
      "version": "0.0.1",
      "description": "Node JS Starter - It beginns",
      "main": "server.js",
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
      },
      "keywords": [
        "Node",
        "Express"
      ],
      "author": "Your Name",
      "license": "MIT",
      "dependencies": {
        "body-parser": "^1.9.2",
        "errorhandler": "^1.2.2",
        "express": "^4.10.2",
        "express-session": "^1.9.1",
        "jade": "^1.7.0",
        "method-override": "^2.3.0",
        "morgan": "^1.5.0",
        "multer": "^0.1.6",
        "serve-favicon": "^2.1.6"
      }
    }
    

* * *

Next lets create our **Server.js** file. Create the file in the root of our *nodejs_startup* folder. Include this code inside our *server.js* file

    var express = require('express');
    var path = require('path');
    
    var favicon = require('serve-favicon');
    var logger = require('morgan');
    var methodOverride = require('method-override');
    var session = require('express-session');
    var bodyParser = require('body-parser');
    var multer = require('multer');
    var errorHandler = require('errorhandler');
    
    /*
    Load controllers
    */
    var homeController = require('./controllers/home');
    
    var app = express();
    
    // all environments
    app.set('port', process.env.PORT || 3000);
    app.set('views', path.join(__dirname, 'views'));
    app.set('view engine', 'jade');
    //app.use(favicon(__dirname + '/public/favicon.ico')); //If you have a favicon then add it here or else leave this commented out.
    app.use(logger('dev'));
    app.use(methodOverride());
    app.use(session({ resave: true,
                     saveUninitialized: true,
                     secret: 'uwotm8' }));
    app.use(bodyParser.json());
    app.use(bodyParser.urlencoded({ extended: true }));
    app.use(multer());
    app.use(express.static(path.join(__dirname, 'public')));
    
    /**
    * Main routes.
    */
    app.get('/', homeController.index);
    
    // error handling middleware should be loaded after the loading the routes
    if ('development' == app.get('env')) {
      app.use(errorHandler());
    }
    
    app.listen(app.get('port'), function(){
      console.log('Express server listening on port ' + app.get('port'));
    });
    

* * *

We will need to create a few files and folders to match our server.js file. Then we will discuss what each line does.

Create these folders **controllers**, **views**, and **public** in the root of our *nodejs_starter* folder.

Inside public create two folders **css** and **js**

Create **home.js** inside our *controllers* folder and this file should contain.

    /**
    * GET /
    * Home page.
    */
    
    exports.index = function(req, res) {
        res.render('home', {
            title: 'Home'
         });
    };
    

Here we are telling express to render *home.jade* and pass in an object with a key of *title* and a value of *Home*. You can use this object in your jade template by using the syntax `#{<key>}`

Next lets create our home.jade file. Add a file called **home.jade** under our views folder, and this file should contain the code below.

    extends layout
    
    block content
      | #{title} page
    

You can see we used our passed in anonymous object and accessed the *title* key to produce the printed value of *Home*.

You can also see `extends layout`. I am not going to go into jade syntax but you can view more [here][3]. However, we will need to create our layout file.

Create a new file called **layout.jade**. This file should contain the code below.

    doctype html
    html
      head
        meta(charset='utf-8')
        meta(http-equiv='X-UA-Compatible', content='IE=edge')
        meta(name='viewport', content='width=device-width, initial-scale=1.0')
        title #{title} - NodeJS Starter
      body
        block content
    

Whats new here? *block content*! - block content displays the template that is called by express.

* * *

Ok lets talk about whats going on in the server.js file now that our folders and files are created.

Line 1-10 : Necessary middleware applications to have your node.js application run correctly with express.

line 15 : Gain access to the home controller and all of the methods needed to create routes for the home page.

line 17: Assign our express app to our app variable.

line 20: Set our port number to run our app. process.env.PORT is used for heroku for production.

line 21: Set the views folder. This folder can be anywhere inside your folder.

line 22-31: Setting session and other middleware items needed to be initialized for express to run properly.

line 32: Set up our static resources file. This is a good place to set up our angularJS files. More on this in another post.

line 37: Set up our initial route for our home page. Here we are using our *controllers* folder and accessing our *home.js* file that contains our home controller. Inside this file is a method of *index*. We are setting "/", which is our main entry point into our app, to use the index method of our home controller. The home controller uses our home.jade view.

Express has 4 main routes. **Get**, **Post**, **Put**, **Delete**. You can view more about express routes [here][4];

line 40 - 46 : Sets up our debugging and starts our Node server.

 [1]: http://nodejs.org/download/
 [2]: http://en.wikipedia.org/wiki/MIT_License
 [3]: http://jade-lang.com/
 [4]: http://expressjs.com/api.html
