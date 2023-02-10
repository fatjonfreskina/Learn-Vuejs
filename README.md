# Learn-Vuejs

## Introduction

When creating a Vue app, you can use the Vue class constructor and the "new" keyword to create a new instance of the Vue class, which can have many properties set through the constructor's single argument.

```js
// app.js
const app = new Vue({
    // parameters
});
```

### el

The Vue constructor only accepts a single argument, which is the options object. The options object contains key-value pairs that provide information to the Vue app. To specify which HTML element the Vue app will interact with, the options object includes a key "el" (representing HTML element) with a value that is a CSS selector string that targets the desired HTML element. This allows the Vue app's logic to access the specified HTML element.

```js
// app.js
const app = new Vue({
  el: '#app' // #id selector
});
```

In the above example, we wanted an HTML element with an ID of app to gain access to our Vue app’s functionality. We added an el key to the options object and made the value '#app', a CSS selector that will target an element with an ID, #, of app.

> This will find the element with an ID of app in the HTML file and transform it into a Vue app.

Adding a `<div>` that surrounds the template code of a Vue app and then using that `<div>` as the value of el is common practice in setting up a Vue app.

### Data

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

### Templates

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

### Directives

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

### Components

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

We use v-bind to set the value of message on the tweet component instance, `<tweet>`, to be the Vue app data‘s value of tweet.

The name of the value after the : is the prop that will be receiving the value. The value of v-bind (the name in quotes) is the data value we will be using to set that prop’s value.

## Data

### Computed properties

Some data can be calculated based on information already stored and doesn’t require it’s own key-value pair in the data object.

Instead of storing computed data as key-value pairs in our data object, we store key-value pairs in a separate computed object. Each key in the computed object is the name of the property and the value is a function that can be used to generate a value rather than a specific value.

```js
// App.js
const app = new Vue({
  el: '#app',
  data: {
    hoursStudied: 274
  },
  computed: {
    languageLevel: function() {
      if (this.hoursStudied < 100) {
        return 'Beginner';
      } else if (this.hoursStudied < 1000) {
        return 'Intermediate';
      } else {
        return 'Expert';
      }
    }
  }
});

// Index.html

<div id="app">
  <p>You have studied for {{ hoursStudied }} hours. You have {{ languageLevel }}-level mastery.</p>
</div>
```

In order to reference a value from data in this function, we treat that value as an instance property using this. followed by the name of the data — in our example, this.hoursStudied.

### Computed Property Setters

Vue allows us to not only determine computed values based on data values but to also update the necessary data values if a computed value ever changes! This allows our users to potentially edit computed values in the front-end and have all of the corresponding data properties get changed in response so that the computed property will re-calculate to the user’s chosen value.

```jsx
// App.js
const app = new Vue({
  el: '#app',
  data: {
    hoursStudied: 274
  },
  computed: {
    languageLevel: {
      get: function() {
        if (this.hoursStudied < 100) {
          return 'Beginner';
        } else if (this.hoursStudied < 1000) {
          return 'Intermediate';
        } else {
          return 'Expert';
        }
      },
      set: function(newLanguageLevel) {
        if (newLanguageLevel === 'Beginner') {
          this.hoursStudied = 0;
        } else if (newLanguageLevel === 'Intermediate') {
          this.hoursStudied = 100;
        } else if (newLanguageLevel === 'Expert') {
          this.hoursStudied = 1000;
        }
      }
    }
  }
});
```

```html
<!--index.html--> 
<div id=“app”>
  <p>You have studied for {{ hoursStudied }} hours. You have {{ languageLevel }}-level mastery.</p>
  <span>Change Level:</span>
  <select v-model="languageLevel">
    <option>Beginner</option>
    <option>Intermediate</option>
    <option>Expert</option>
  </select>
</div>
```

The set function updates other data whenever the value of languageLevel changes. set functions take one parameter, the new value of the computed value. This value can then be used to determine how other information in your app should be updated. In this case, whenever languageLevel changes, we set the value of hoursStudied to be the minimum number of hours needed to achieve that mastery level.

However, there is one caveat. **A computed value will only recompute when a dynamic value used inside of its getter function changes.**

### Watchers

But what do we do if we want to make app updates without explicitly using a value in a computed function? We use the watch property.

```jsx
const app = new Vue({
  el: '#app',
  data: {
    currentLanguage: 'Spanish',
    supportedLanguages: ['Spanish', 'Italian', 'Arabic'],
    hoursStudied: 274
  },
  watch: {
    currentLanguage: function (newCurrentLanguage, oldCurrentLanguage) {
      if (supportedLanguages.includes(newCurrentLanguage)) {
        this.hoursStudied = 0;
      } else {
        this.currentLanguage = oldCurrentLanguage;
      }
    }
  }
});
```

In this example, we want to set the number of hours studied to 0 whenever the user changes languages to a new supported language. If the language is not supported, it reverts the language back to the previously-selected language.

This functionality is not a computed value because we do not actually need to continually use this information to compute a new dynamic property: we just need to update existing properties whenever the change happens.

The value of watch is an object containing all of the properties to watch. The keys of this object are the names of the properties to watch for changes and the values are functions to run whenever the corresponding properties change. These functions take two parameters: the new value of that property and the previous value of that property.

**Note**: It may seem like you could use watch in many instances where you could use computed. The Vue team encourages developers to use computed in these situations as **computed values update more efficiently than watched values**.

### Instance Methods

The methods property is where Vue apps store their instance methods. These methods can be used in many situations, such as helper functions used in other methods or event handlers (functions that are called in response to specific user interactions).

```js
const app = new Vue({
  el: "#app",
  data: {
    hoursStudied: 300
  },
  methods: {
    resetProgress: function () {
      this.hoursStudied = 0;
    }
  }
});
```

```html
<button v-on:click="resetProgress">Reset Progress</button>
```

TODO: UNDERSTAND COMPUTED AND WATCH.
TODO: copy the project section by section in this repo
