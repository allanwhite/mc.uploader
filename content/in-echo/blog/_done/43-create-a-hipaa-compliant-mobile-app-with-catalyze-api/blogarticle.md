---
Title: Create a HIPAA Compliant Mobile Application with Ionic and the Catalyze API
SeoTitle: Create a HIPAA Compliant Mobile Application with Ionic and the Catalyze API
Author: anthony
Date: 06/19/2014
Summary: 
Lead: |
  Let's cut to the chase. You want to create a mobile application that stores PHI (protected health information), and you want to get it to users as fast as possible. A good option for reaching your customers quickly, without spending time becoming a HIPAA expert, is by using the Catalyze HIPAA Compliant Backend as a Service (BaaS) for storing your data and [Ionic](http://ionicframework.com/) for developing your mobile app.

Tags: code, tutorial, mobile, ionic
Category: company
Fullname: Anthony Pleshek
---
In this post, I'll explain the basics of getting set up with Catalyze and Ionic by explaining how to create a simple app with Ionic that can be used to sign in with a Catalyze account.

### Set up your environment

To get started, you'll need to sign up for and activate an account on the [Catalyze dashboard](https://dashboard.catalyze.io/signup) if you haven't already done so. You'll also need to get all of the necessary components to develop using the Ionic Framework. Following their [getting started guide](http://ionicframework.com/getting-started/) will get you everything you need to begin. I used the blank project template for the code below, but there shouldn't be too many changes to use the tabs or sidemenu templates.

Ionic is heavily tied to [angular.js](https://angularjs.org/), so if you don't have any experience with angular, you might want to do a little up front research before jumping in to writing your code. A good overview is [here](http://stephanebegaudeau.tumblr.com/post/48776908163/everything-you-need-to-understand-to-start-with).

In order to interact with the Catalyze API, you'll need to get an API Key. The easiest way to do so is through the [dashboard](https://dashboard.catalyze.io). If you've just created an account, you'll need to [create an organization](https://dashboard.catalyze.io/account) for your account, [create an application](https://dashboard.catalyze.io/applications), and create an API Key for your application.

Make sure to keep your browser open or take note of you application id and the API key that was created as you will need them shortly.

The easiest way to interact with the Catalyze API is to use the [Catalyze Javascript SDK](https://github.com/catalyzeio/catalyze-js-sdk). For ionic, you will probably want to install it using bower, and save it to your dependencies by running `bower install --save-dev catalyze` from the command line within your ionic project's directory.

### Set up the controllers

For the purposes of this tutorial, we're going to only create a single file to keep track of our controllers. The file www/js/controllers.js should contain the following:

```
//Create an angular module. Keeping it basic and naming this one 'controllers'
angular.module('controllers', [])

.controller('SignInCtrl', function($rootScope, $scope, $state, catalyze) {

	$scope.signInUser = function(username, password) {
		catalyze.signIn(username, password, function(d) {
			$rootScope.user = d;
			$rootScope.sessionToken = d.sessionToken;
			$state.go('sign-in-success');
		}, function(result) {
			angular.forEach(result.errors, function(error){
				console.log(error.message);
			});
		});
	}
})

.controller('SignInSuccessCtrl', function() {
});
```

### Inject dependencies into our app

Before using our controllers modules and the Catalyze Javascript SDK, we must let angular know about them. To do this, we must include the js files in our index.html and then add 'controllers' and 'catalyze' to the list of modules injected into the app in app.js.

The line creating the app module in app.js should look like this:

	angular.module('starter', ['ionic', 'controllers', 'catalyze'])

Add the following lines to index.html. I put the line for the catalyze-js-sdk directly underneath the line for ionic.bundle.js, and controllers directly underneath the line for app.js:

	<script src="lib/catalyze/dist/angular-catalyze.js"></script>

	<script src="js/controllers.js"></script>

One more change to index.html is to add in an ion-nav-view in place of ion-content. This will contain the views that will be created in the next step. The body of index.html should look like this:

	<body ng-app="starter">
		<ion-pane>
			<ion-header-bar class="bar-stable">
				<h1 class="title">Ionic Blank Starter</h1>
			</ion-header-bar>
			<ion-nav-view></ion-nav-view>
		</ion-pane>
	</body>

### Create the views

There are two views I'm going to create: a view for the sign in form, and a sign in success view. To start with, I created a new directory and files for the two new views.

```
www/
- js/
-- templates/
--- sign-in.html
--- sign-in-success.html
```

##### sign-in.html

```
<ion-view title="Sign In">
  <ion-content class="has-header padding">
    <h1>Sign In</h1>
		<form role="form" ng-submit="signInUser(username, password)" ng-controller="SignInCtrl">
			<div>
				<label for="username">Username</label>
				<input name="username" ng-model="username" placeholder="username">
			</div>
			<div>
				<label for="password">Password</label>
				<input name="password" ng-model="password" type="password" placeholder="password">
			</div>
			<button type="submit">Sign In</button>
		</form>
  </ion-content>
</ion-view>
```

##### sign-in-success.html

```
<ion-view title="Sign In Success">
  <ion-content class="has-header padding">
    <h1>Signed In Successfully!</h1>
  </ion-content>
</ion-view>
```

### State configuration

The last thing we need to do is tell our app how to deal with the different states it can be in. For handling states, keep this [link](https://github.com/angular-ui/ui-router) handy.

Add the following to the module in app.js:

```
.config(function($stateProvider, $urlRouterProvider) {

	// Ionic uses AngularUI Router which uses the concept of states
	// Learn more here: https://github.com/angular-ui/ui-router
	$stateProvider

		.state('sign-in', {
			url: "/sign-in", 
			templateUrl: 'templates/sign-in.html', 
			controller: 'SignInCtrl'
		})

		.state('sign-in-success', {
			url: "/sign-in-success", 
			templateUrl: "templates/sign-in-success.html", 
			controller: "SignInSuccessCtrl"
		});

	// if none of the states match, go to sign-in
	$urlRouterProvider.otherwise('/sign-in');
})
```

Finally, we need to tell our ionic app what our Catalyze application's app id and API key are. Edit the .run($ionicPlatform) section to look like the code below, making sure to enter in the application id and API key you created earlier.

```
.run(function($ionicPlatform, catalyze) {

	//Configure the catalyze service with your Catalyze application's information
	catalyze.config({
		apiKey: "YOUR API KEY", 
		appId: "YOUR APPLICATION ID"
	});

	$ionicPlatform.ready(function() {
		// Hide the accessory bar by default (remove this to show the accessory bar above the keyboard
		// for form inputs)
		if(window.cordova && window.cordova.plugins.Keyboard) {
			cordova.plugins.Keyboard.hideKeyboardAccessoryBar(true);
		}
		if(window.StatusBar) {
			StatusBar.styleDefault();
		}
	});
})
```

### Test it out

Now that everything is in place, you should be able to run the command `ionic serve` from a terminal, and test the app in your browser. You should be able to log in with your Catalyze dashboard account; any login errors that occur should be viewable with your browser's developer tools. NOTE: Make sure you run the command from the top directory of your project, not within the www directory.

### Final notes

Some corners were cut while writing this post to keep it as short as possible while still hitting the key points, so before you dive into development, make sure you structure your project appropriately. In most cases, having all of your services in services.js and all of your controllers in controllers.js will lead to headaches down the road. I would suggest checking out this [link](http://blog.angularjs.org/2014/02/an-angularjs-style-guide-and-best.html), particularly the 'Best Practice Recommendations' link therein.

Overall we love working with Ionic, and plan to do a lot more with it in the future. HIPAA compliance is a headache, as is writing mobile apps to run on multiple mobile platforms; combining Ionic and Catalyze is a great way to overcome both and get your compliant app built and deployed quickly.

