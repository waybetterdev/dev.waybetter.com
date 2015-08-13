---
title: Building an AngularJS app
tags: [angular, frontend, jason_savino]
name: [Jason Savino]
author: [jason_savino]

---

This post will walk you through how to get an AngularJS client-side theme running on an existing Drupal 8 app server. We will discuss setting up the Drupal 8 server in a future post.

When complete we will have an AngularJS site that can connect to any Drupal 8 server with only a few customizations. You will be able to pull the code from Github HERE.

## Setting up your local environment
The setup can be made simple by using some popular tools and following best practices.
- Homebrew
- NodeJS
- Yeoman
- Grunt
- Bower
- Angular App Generator

If you have all of these installed already, skip to the "Building the App" section.

To start, we will install Homebrew. Simply paste the line of code below into your terminal's command line and follow the prompts.

	$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"


Next we will need to install NodeJS on your system. Since we have Homebrew running we can use the following command:

	$ brew install node

Now that node is installed we can npm to grab Yeoman, Grunt, Bower and Angular app generator with one line of code:

	$ npm install -g yo grunt-cli bower generator-angular

That's it! Your system now has all the tools you need to build the app.

## Building the App

First, let's create a project directory and go into it:

	$ mkdir drupal-app && cd $_

Second, we will create the angular app. After running the command below, just hit enter to confirm the setup defaults:

	$ yo angular drupalClient

Everything you need has now been created for you:

You now have all the SASS, JS and HTML files you need. Grunt will be used to monitor the directory for any changes and will even refresh the page for you.

On the initial install Grunt is used to start the app server. You can use `CTL+C` to stop the server. Anytime you want to start it back up just run the following from your project directory:

	$ grunt serve

## Creating the Display
To start, we will connect the app to an existing Drupal 8 View with the URL /nodes and create a list of movie teasers. While we can create the files we need manually, we have Yeoman and generator-angular so let's allow them to do the work:

	$ yo angular:route nodes

This will generate the the controller, the view and the test controller files:

	app/scripts/controllers/nodes.js
	test/spec/controllers/nodes.js
	app/views/nodes.html

as well as add a route to the `app/scripts/app.js` file that looks like this:

~~~javascript
.when('/nodes', {
  templateUrl: 'views/nodes.html',
  controller: 'NodesCtrl'
})
~~~

We will want to let the app know where we are for navigation purposes later so we need to edit the file as follows:
~~~javascript
.when('/nodes', {
  templateUrl: 'views/nodes.html',
  controller: 'NodesCtrl'
})
~~~
also adding some code so we can specify the Drupal 8 server URL in a variable, in this case I am using `d8server.local`:

~~~javascript
.config(function ($routeProvider) {
  ...
})
.run(function($rootScope) {
  $rootScope.baseUrl = 'http://d8server.local';
});
~~~

In the `app/scripts/controllers/node.js` file, you should make this change:

~~~javascript
angular.module('drupalClientApp')
  .controller('NodesCtrl', function ($scope, $rootScope, $http) {
    $scope.nodes = [];
    $http.get($rootScope.baseUrl + '/nodes').success(function(result){
      console.log('NodeCtrl GET result', result);
      $scope.nodes = result;
    });
  });
~~~

We made some big changes to the original file. First we added two arguments to the controller function; `$rootScope` and `$http` so we can use them in our function. Next we create an array names nodes to hold our results and expose them to our HTML templates. Lastly, we used the $http object's get method to call our Drupal View. Notice the $rootScope.baseUrl that we populated in the `app/scripts/app.js` file? This makes maintenance much easier. 

Take some time and use the console to drill down into the result object. 

Now we can use this info to edit the `app/views/nodes.html` template:

~~~javascript
<h1>Movie List</h1>
<article class="" ng-repeat="node in nodes">
    <h2>
      <a ng-href="#/main/{{ node.uuid[0].value }}">
        {{ node.title[0].value }}
      </a>
    </h2>
    <p ng-bind-html="node.body[0].value"></p>
</article>
~~~

Here we use Angular's `ng-repeat` to loop through the nodes array we added to the $scope variable and make a list of teasers.
We create an anchor using ng-href to build our link (we will discuss the data in another post) and wrap that around the node's title. Then we use a special ng-bind-html as a placeholder for the node's body since it contains markup.

The result:
NEED way to embed imgs in repo. 
https://github.com/sculpin/sculpin/issues/118

There you have it! We have successfully created a AngularJS client-side theme that can connect to an existing Drupal 8 app server.
In my next post we will learn how to set up Drupal 8 to server our REST data. We will also configure the AngularJS app templates to display single nodes.

Happy coding!!
