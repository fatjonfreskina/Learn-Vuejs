# Learn-Vuejs

By invoking the Vue class constructor with the new keyword, we create a new instance of the Vue class which we name app. The Vue constructor can set many properties on our Vue app when it is called. However, unlike many constructors, the Vue class does not take each of these properties as separate arguments. Rather, it only takes one argument.

```js
// app.js
const app = new Vue({
    // parameters
});
```

## el

The Vue constructor takes in only one object, called the options object. Each piece of information the Vue app needs to function is added to the options object as a key-value pair. This means that developers can easily update or add information in the Vue app by just looking for the correct key in the options object.

We need to specify to our Vue app which portion of our HTML we want to gain access to our Vue app’s logic.

We do this by adding a key-value pair to the Vue app’s options object. We add a key called el, standing for **HTML element**, with a value of a CSS selector as a string that will target an element in our HTML and give it access to our Vue app’s functionality.

```js
// app.js
const app = new Vue({
  el: '#app' // #id selector
});
```

In the above example, we wanted an HTML element with an ID of app to gain access to our Vue app’s functionality. We added an el key to the options object and made the value '#app', a CSS selector that will target an element with an ID, #, of app.

## Data

An essential feature of all front-end frameworks is rendering and updating dynamic data. Information like the number of likes on a social media post may change at any second. Front-end frameworks must make it easy to display these types of dynamic data and automatically update them for users as soon as they change.

Therefore, all of our dynamic data will need to be specified in our options object at a property called data.

```js
const app = new Vue({
  el: '#app',
  data: {
    username: 'Michael'
  }
});
```

Apps need to display many pieces of dynamic data. To accommodate this, the value of .data is an object as well.

## Templates

We now know that we store our dynamic information on the .data attribute of our Vue app options object, but how do we display that information if it potentially will keep changing values?

Vue makes use of **templating**, meaning that the developer specifies certain content in our HTML that isn’t meant to be displayed literally but rather substituted out with the appropriate data from the app.

We specify which content inside our HTML should be substituted by surrounding it in two layers of curly brackets, like so:

```js
<div id="app">
  <h2>Hello, {{ username }}</h2>
</div>
```

In this example, {{ username }} will be filled in with the value of username from the Vue app’s .data object when the page is rendered to the user. If the value of username changes, the value displayed to the user will be changed as well.

Whenever you want to display information from the Vue app’s data, you wrap the name of the .data property in two sets of mustaches (curly brackets) and the expression will be replaced with the Vue data information for the end user to see.

## Directives

Directives are custom HTML attributes built into Vue that accomplish incredibly complex, common front-end operations using barely any code.

For example, one very common front-end need is to conditionally display elements. Let’s say we only want to show a login button if a user isn’t already logged in. We can add a **v-if** directive as an attribute to HTML elements like so:

```jsx
<button v-if="userIsLoggedIn">Log Out</button>
<button v-if="!userIsLoggedIn">Log In</button>
```

In this case, it will check our .data for a value of userIsLoggedIn. Then it will only display our “Log Out” button if userIsLoggedIn is true and will only display our “Log In” button if it is false.

Another complex, common front-end need is to render an array of items identically. We can use **v-for** as an attribute, like so:

```jsx
<ul>
  <li v-for="todo in todoList">{{ todo }}</li>
</ul>
```

One more super cool directive is **v-model**. v-model can be added to any form field and hooked up to our Vue app’s data. Modifying the form field will then automatically modify the specified Vue app data!

```jsx
<input v-model="username" />
```

The above input field will display the current value of username on the Vue app’s data object and will change the value of username if the user modifies the value in the field. That’s some complicated JavaScript implemented perfectly with very little code.

**v-on:click** takes JavaScript code as its value: `<button v-on:click="tweets.push(newTweet)">Add Tweet</button>`.

## Components

Vue has added the ability to create custom, reusable HTML elements called components.

When creating a component, you provide a template that should be rendered whenever the component is used in HTML. You then specify which **pieces of dynamic information, called props**, the component can receive to fill in this template.

When used in your HTML code, props look like normal HTML attributes, you add them to the opening tag of the component HTML element with a name and a value.

Once you’ve created your component, you can then use it throughout your site just like any other HTML element. This means no copy/pasting of HTML code, no need to make the same change in multiple places across your site, and no potentially broken or misstyled elements.

```jsx
// Tweet.js
const Tweet = Vue.component('tweet', {
 props: ['message','author'],
 template: '<div class="tweet"><h3>{{ author }}</h3><p>{{ message }}</p></div>'
});
```

Finally, let’s pass in the value of data‘s username to the author prop on our component using the v-bind directive.

v-bind takes a value from the Vue app’s data object and uses it as the value of the specified component prop.

```html
<div class="tweets">
    <tweet v-for="tweet in tweets" 
            v-bind:message="tweet"
            v-bind:author="username"></tweet>
</div>
```

We use v-bind to set the value of message on the tweet component instance, <tweet>, to be the Vue app data‘s value of tweet.

The name of the value after the : is the prop that will be receiving the value. The value of v-bind (the name in quotes) is the data value we will be using to set that prop’s value.


