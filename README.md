# nodejs-on-heroku

A nodejs appdeployed on heroku

# Reference
This comes from http://www.jamesward.com/2011/06/21/getting-started-with-node-js-on-the-cloud/

====================

Step 1) Sign up for Heroku

Step 2) Install the Heroku command line client

Step 3) Login to Heroku via the command line:

	heroku auth:login

Step 4) Install git

Step 5) Install Node.js
	
	On Ubuntu
	sudo apt-get install python-software-properties
	sudo add-apt-repository ppa:chris-lea/node.js
	sudo apt-get update
	sudo apt-get install nodejs npm

Step 6) Create a Node.js app

	I started by building a very simple “hello, world” Node.js app. In a new project directory I created two new files. First is the package.json file which specifies the app metadata and dependencies:
	
	{
	  "name": "hellonode",
	  "version": "0.0.1",
	  "dependencies": {
	    "express": "2.5.11"
	  },
	  "engines": {
	    "node": "0.8.4",
	    "npm": "1.1.45"
	  }
	}
	
	Then the actual app itself contained in a file named web.js:
	
	var express = require('express');
	 
	var app = express.createServer(express.logger());
	 
	app.get('/', function(request, response) {
	  response.send('hello, world');
	});
	 
	var port = process.env.PORT || 3000;
	console.log("Listening on " + port);
	 
	app.listen(port);
	This app simply maps requests to “/” to a function that sends a simple string back in the response. You will notice that the port to listen on will first try to see if it has been specified through an environment variable and then fallback to port 3000. This is important because Heroku can tell our app to run on a different port just by giving it an environment variable.

Step 7) Install the app dependencies with npm:

	npm install .
	This uses the package.json file to figure out what dependencies the app needs and then copies them into a “node_modules” directory.

Step 8) Try to run the app locally:

	node web.js
	You should see “Listening on 3000″ to indicate that the Node.js app is running! Try to open it in your browser:
	http://localhost:3000/
	
	Hopefully you will see “hello, world”.

Step 9) Heroku uses a “Procfile” to determine how to actually run your app. Here I will just use a Procfile to tell Heroku what to run in the “web” process. But the Procfile is really the foundation for telling Heroku how to run your stuff. I won’t go into detail here since Adam Wiggins has done a great blog post about the purpose and use of a Procfile. Create a file named “Procfile” in the project directory with the following contents:

	web: node web.js
	This will instruct Heroku to run the web app using the node command and the web.js file as the main app. Heroku can also run workers (non-web apps) but for now we will just deal with web processes.

Step 10) In order to send the app to Heroku the files must be in a local git repository. To create the local git repo, run the following inside of your project directory:
	
	git init
	Now add the three files you’ve created to the git repo:
	
	git add package.json Procfile web.js
	Note: Make sure you don’t add the node_modules directory to the git repo! You can have git ignore it by creating a .gitignore file containing just “node_modules”.
	
	Commit the files to the local repo:
	
	git commit -m "initial commit"

Step 11) Create an app on Heroku:
	
	heroku create
	A default / random app name is automatically assigned to your app.

Step 12) Now you can push your app to Heroku! Just run:

	git push heroku master

