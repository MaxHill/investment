# Vue-starter - client
> An opinionated **Vue**-starter setup with **karma**, **jasmine** and **nightwatch.js** for testing. **Es6** transpilation with **babel**. And some **browserify** magic to tie it all together.

## Table of contents

* [Usage / installation](#usage-installation)
* [Actions](#actions)
* [Generators](#generators)
* [Testing](#testing)
* [What goes where?](#what-goes-where)

## Usage / installation

This is a project template for [vue-cli](https://github.com/vuejs/vue-cli).

``` bash
# Create new project
$ npm install -g vue-cli	#only needed first time
$ vue init MaxHill/investment my-project

# Install your project
$ cd my-project
$ git init	#needed for pre-commit hook
$ npm install

# Run your project
$ npm start
# or
$ gulp
```

## Actions
`$ npm start` / `$ gulp` Run this when you want to develop. Starts the *dev server*, *watchers* and *transpilers* etc.

`$ npm test` Runs the unit tests once (this is done automatically when running gulp)

## Generators
Generators are scripts that generate file-stubs. Generators never add to, or override existing files! You therefore need to require the files where you want to use them. So if you generated a page, you will then need to add it to the router (`app/js/app.js`) manually.

**Tip:** Create aliases to be able to quickly run the generators. Add the following to `~/.baschrc`.

~~~bash
# Mdb - generators
alias g:p="npm run-script generate:page"
alias g:c="npm run-script generate:component"
alias g:m="npm run-script generate:mixin"
alias g:t="npm run-script generate:test"
~~~

### Page

~~~bash
$ g:p --name="about"
# or
$ npm run-script generate:page --name="about"
~~~

This wil create both a template file and a page component file.
Dont forget to add the page component file to the router in `app/js/routes.js` :

~~~js
module.exports = {
    '/about': {
        component: require('./views/about')
    }
};
~~~

### Component
~~~bash
$ g:c --name="navigation"`
# or
$ npm run-script generate:component --name="navigation"
~~~

This wil create both a template file and a component file.

### Mixin
~~~bash
$ g:m --name="canAuthenticate"`
# or
$ npm run-script generate:mixin --name="canAuthenticate"
~~~

This will create a mixin file.

### Test
~~~bash
$ g:t --name="navigation"
# or
$ npm run-script generate:test --name="navigation"
~~~

This will create a test stub.
## Testing
### End to end
End to End testing is setup using **nightwatch.js** and uses the selenium driver which depends on java. Therefore you might have to update your java version if you run in to problems with version mismatch.

To run the e2e tests run the following command in your terminal:
~~~bash
$ gulp e2e
~~~

You can find an example e2e-test in the `test/e2e/` folder.

### Unit
Unit-testing is setup with the **jasmine** as the testing framework and the **karma** as the test runner.
You can find an example test in the `/test` folder.

You can generate a new test stub with the [generate:test command](#tests) or manually create a new test file.
For your conveniance there is a `test-helper.js` file with methods for bootstraping components or mixins with a clean vue instance.
It's a good prtice to extract common testing methods to this file.

### Testing components / mixins
To test components and mixins we need to bootstrap them with a vue instance. As stated earlier this project includes the helper functions "bootstrapComponent" & "bootstrapMixin". These methods can be called before each assertion.
Use them like this:
~~~js
describe('About component:', () => {
    var component;
    beforeEach(() => {
        component = Help.bootstrapComponent(require('path/to/component'));
    });
~~~
#### Here follows an example of how you might test an example component.
Explanation follows below example.

**Generate the component and test:**

~~~bash
$ g:c --name="greeting"
$ g:t --name="greeting"
# or
$ npm run generate:test --name="greeting"
$ npm run generate:test --name="greeting"
~~~

**And write the following in the generated files:**

~~~js
//app/js/components/greeting.js

module.exports = {
	template: require('./greeting.template.html'),
	data: {
		return {
			names:['John Doe']
		}
	},
	methods() {
		addName(name) {
			this.names.push(name)
		}
	}
}
~~~

~~~html
<!-- app/js/components/greeting.template.html -->

<div>
	<ul>
		<li v-for="name in names">Hello {{ name }}!</li>
	</ul>
	<button @click="addName('world')">Say hello to the world</button>
</div>
~~~

~~~js
//app/test/greeting.test.js

var Help = require('./test-helper.js');

describe('Greeting component', () => {

	// Bootstrap the component before each assertion runs.
    var greeting;
    beforeEach(() => {
        greeting =
			Help.bootstrapComponent(require('../app/js/components/greeting'));
    });

	/*
	* Assert the component exists and is object.
	*/
    it('should exist', () => {
        expect(typeof greeting).toBe('object');
    });

	/*
	* Assert that some data is set correctly on the component.
	*/
    it('should initialy have the names attribute set correctly', () => {
        expect(greeting.names).toBe(['John Doe']);
    });

	/*
	* Assert you can call a method to add a name to the name list.
	*/
    it('should be able to add a name to the name list', () => {
        expect(greeting.names).toBe(['John Doe']);

        greeting.addName('Test Person');

        expect(greeting.name).toBe(['John Doe', 'Test Person']);
    });

});
~~~

Make sure it all works by running `$ npm test`.

#### Woa! What is happening here? Let's dig in!

Well, first we define our basic greeting component that will display an unordered list of greetings. The next thing we do is define the template for said component. Notice the button for adding world to the list of names we want to greet.

Now to the actual test. The first thing we do is include the test-helper file. This is the file that holds our bootstrap methods we talked about earlier. After that we start the test by saying we want to describe the "Greeting component". The "beforeEach" method will be called before each assertion (`it('should...`). Here we use our helper method to bootstrap our component. This is so we in each assertion will have a fresh copy of our component.

We begin with testing that the component exist by making sure the type is object. We proceed with making sure the component has the correct default data. Finally we make sure we can add a name to the list of names.

## What goes where?

* app/
  * [js/](#js)
    * [components/](#components)
    * [mixins/](#mixins)
    * [views/](#views)
    * [app.js](#appjs)
	* [config-dev.js](#config-devjs)
	* [config.js](#configjs)
    * [routes.js](#routesjs)
	* [vue-register.js](#vue-registerjs)
* [commands/](#commands)
* [bin/](#bin)
* [build-tasks/](#build-tasks)
* [public/](#public)
* [test/](#test)
    * [e2e/](#e2e)

#### Js
This is where you write all your js code.

#### Components
Components are reusable parts of pages, like navigation,footer or maby a button.
You may use components in pages as well as in other components.

#### Mixins
Some components will share some functionality. If thats the case extracting a mixin might be helpfull.
Think of this lika a php trait if you want.

#### Views
These are the different views or "pages" of your application. It can for example be the /about page.

#### App.js
This is your main javascript file where everything is required and setup. We register our root vue instance and router. Finally we boot the application.

#### config.js
This is where you keep your production configuration variables. For example app-id live url or what have you. 

#### config-dev.js
This is where you keep your development configuration variables. Overrides config.js options. Create this if you need it.

#### vue-register.js
This is where your root vue instance is setup. This is where plugins are setup. Why we do this in a separate file is because we want to be able to use this file in our tests aswell.

#### Routes.js
This is just a simple javascript object where you specify your routes. Nothing more. Nothing less.

#### Commands
You probably don't need to worry about this folder. This is where your commandline commands live. For example the generators.

#### Public
This is the application that is build by the gulp file. This is all the browser knows about.

#### Test
This is the most sacred place in your application. Here you'll write all of your tests. Because you do write tests, right? ;)

#### e2e
To compliment your unit tests you can write e2e (finctional/integration) tests in this folder.
