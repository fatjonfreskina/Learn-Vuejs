# Learn-Vuejs

## Index

- [Introduction](#introduction)
- [Data](#data)
- [Vue Forms](#vue-forms)
- [Styling elements](#styling-elements)

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

### Data property

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

One more super cool directive is **v-model**. v-model can be added to any **form field** and hooked up to our Vue app’s data. Modifying the form field will then automatically modify the specified Vue app data!

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

With Options API, we use the data option to declare reactive state of a component. The option value should be a function that returns an object. Vue will call the function when creating a new component instance, and wrap the returned object in its reactivity system.

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
<div id="app"> 
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

The value of watch is an object containing all of the properties to watch. **The keys of this object are the names of the properties to watch for changes and the values are functions to run whenever the corresponding properties change**. These functions take two parameters: the new value of that property and the previous value of that property.

**Note**: It may seem like you could use watch in many instances where you could use computed. The Vue team encourages developers to use computed in these situations as **computed values update more efficiently than watched values**.

> If possible : use computed.

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

## Vue Forms

In this lesson, we will:

- Bind all of the form fields to our Vue data
- Implement the functionality to reset and submit our form and clean up our form data
- Implement form validation to ensure users are using our form as expected

### Radio Button Bindings

An interesting example of a slightly more complex form field is radio buttons. Radio buttons are a series of buttons where the user can only select one. When a different button is selected, the previously-selected button becomes unselected.

In HTML, each radio button is its own \<input> field. However, they all correspond to the same piece of data in the Vue app. As a result, each \<input> field will need its own v-model directive. However, the value of v-model for each \<input> will be the same: the name of the property they all correspond to.

```jsx
// Index.html
<legend>How was your experience?</legend>
 
<input type="radio" id="goodReview" value="good" v-model="experienceReview" />
<label for="goodReview">Good</label>
 
<input type="radio" id="neutralReview" value="neutral" v-model="experienceReview" />
<label for="neutralReview">Neutral</label>
 
<input type="radio" id="badReview" value="bad" v-model="experienceReview" />
<label for="badReview">Bad</label>

// App.js

const app = new Vue({ 
  el: '#app', 
  data: { experienceReview: '' } 
});
```

### Checkboxes

Unlike radio buttons, previous selections won’t be unselected when new selections are added. Instead, users can select all of the relevant checkboxes they’d like.

As a result, the dynamic piece of data bound to these types of checkboxes must be an array.

Example:

```html

<!-- index.html -->

<legend>Which of the following features would you like to see added?</legend>
 
<input type="checkbox" id="search-bar" value="search" v-model="requestedFeatures">
<label for="search-bar">Search Bar</label>
 
<input type="checkbox" id="ads" value="ads" v-model="requestedFeatures">
<label for="ads">Ads</label>
 
<input type="checkbox" id="new-content" value="content" v-model="requestedFeatures">
<label for="new-content">New Content</label>
```

```js
// app.js

const app = new Vue({ 
  el: '#app', 
  data: { requestedFeatures: [] } 
});
```

### Boolean checkbox

A single checkbox, however, can be represented by a boolean value. If the checkbox is checked, the value is true — if the value is unchecked, the value is false.

```html
<legend>Would you recommend this site to a friend?</legend>
<input type="checkbox" v-model="wouldRecommend" />
```

```js
const app = new Vue({ 
  el: '#app',
  data: { wouldRecommend: false } 
});
```

### Form event handlers

As we saw briefly in Introduction to Vue, Vue uses the `v-on` directive to add event handlers. Event handlers will respond to the specified event by calling the specified method.

We can use the v-on directive on \<form> elements to add form event handling functionality, like so:

```js
<form v-on:reset="resetForm">
  ...
  <button type="reset">Reset</button>
</form>
```

```js
const app = new Vue({ 
  el: '#app', 
  methods: { resetForm: function() { ... } }
});
```

In this example, we added a reset event handler to our form. We specify the type of event to respond to after a colon, :, and then specify the method to call as the value of the directive. When a user clicks the “Reset” button, a reset event will be triggered (because the type of the button is reset), the \<form> event handler will see this event appear, and the resetForm method will be called in response.

> Note: A common shorthand for event handlers involves **replacing v-on: with @**, like so: `<form @reset="resetForm">...</form>`

Why using @reset when html already has the reset button type?

- Additional logic
- Custom behavior
- Consistency with vue: keep yoor logic consistent with other vue patterns
- Clear separation of concerns: more maintanable in the future

### Form event modifiers

In order to ensure a great web experience, browsers set up default actions to perform in response to events. You saw this in the previous exercise when your app refreshed the page in response to a form submit event.

Event objects have built-in methods to modify this behavior, such as `Event.preventDefault()` (which stops the browser from performing its default event-handling behavior) and `Event.stopPropagation()` (which stops the event from continuing to be handled beyond the current handler).

Vue gives developers access to these methods in the form of modifiers. Modifiers are properties that can be added to directives to change their behavior. Vue includes modifiers for many common front-end operations, such as event handling.

Example

```html
<form v-on:submit.prevent="submitForm">
  ...
</form>
```

In this example, we added the prevent modifier to a form submit event handler directive. This will automatically call Event.preventDefault() whenever our event handler is triggered — in the case of form submit events, this will prevent the page from reloading.

### Input modifiers

We can use modifiers to make our form fields even more versatile.

Vue offers the following three modifiers for v-model:

- .number — automatically converts the value in the form field to a number (to save the appropriate data type and not have to convert it later)
- .trim — removes whitespace from the beginning and ends of the form field value
- .lazy — only updates data values when change events are triggered (often when a user moves away from the form field rather than after every keystroke)

### Form Validation

There are many ways to implement form validation in Vue — we will cover one of the more common methods.

This method makes heavy use of the disabled \<button> property. In brief, if disabled is present (or set to true) on a \<button> element, that \<button> will not do anything when pressed. Whereas if disabled is not present (or set to false), the button will work as expected.

Example

```html
<button type="submit" v-bind:disabled="!formIsValid">Submit</button>
```

```js
const app = new Vue({ 
  el: '#app', 
  computed: { 
    formIsValid: function() { ... } 
  }
});
```

Here we:

- We use the v-bind directive to set the value of the disabled property on a “Submit” button to the value of a computed property called formIsValid
- formIsValid will contain some logic that checks the values stored on the Vue app and returns a boolean representing whether or not the form is valid
- If the form is valid, formIsValid will return true and the disabled property will not be present on the “Submit” button, keeping the button enabled
- If the form is not valid, formIsValid will return false and the button will be disabled.

## Styling elements

### Inline dynamic styling

The name of this course may be slightly misleading — the only way to style HTML elements is still CSS. However, now that we know how to create dynamic web pages, we need our CSS to dynamically change too.

In the past, we have advocated against the use of inline styles, since they make CSS harder to debug and make HTML harder to read. However, front-end frameworks actually make inline styling a powerful tool due to their ability to contain CSS to small, reusable pieces of front-end code. We will use them extensively later on in your Vue journey.

```jsx
// index.html
<h2 v-bind:style="{ color: breakingNewsColor, 'font-size': breakingNewsFontSize }">Breaking News</h2>

// app.js
const app = new Vue({ 
  data: { 
    breakingNewsColor: 'red',
    breakingNewsFontSize: '32px'
  }
});
```

Note in the example that if we want to set a value for a hyphenated CSS property (con trattino!), such as font-size, we need to put the property name in quotes in order to construct a valid JavaScript object.

This example only used values stored in data, however computed properties can be used for v-bind:style and all of the other directives covered in this lesson in the same way.

### Computed Style Objects

A common pattern for adding dynamic inline style objects is to add a dynamic Vue app property that generates the style object. For example, we could refactor the previous example as follows:

```jsx
// index.html
<h2 v-bind:style="breakingNewsStyles">Breaking News</h2>

// app.js
const app = new Vue({ 
  data: { 
    breakingNewsStyles: { 
      color: 'red',
      'font-size': '32px'
    }
  }
});
```

### Multiple Style Objects

Another powerful aspect of v-bind:style is that it can also take an array of style objects as a value.

```jsx
// index.html
<h2 v-bind:style="[newsHeaderStyles, breakingNewsStyles]">Breaking News</h2>

// app.js
const app = new Vue({ 
  data: {
    newsHeaderStyles: { 
      'font-weight': 'bold', 
      color: 'grey'
    },
    breakingNewsStyles: { 
      color: 'red'
    }
  }
});
```

However, this approach easily produces difficult-to-read code (Vue data property explodes)..

### Classes

```jsx
// index.html
<span v-bind:class="{ unread: hasNotifications }">Notifications</span>

// style.css
.unread {
  background-color: blue;
}

// app.js
const app = new Vue({
  data: { notifications: [ ... ] },
  computed: {
    hasNotifications: function() {
      return notifications.length > 0;
    }
  }
})
```

In this example, we are using the v-bind:class directive to dynamically add a class called unread to a “Notifications” \<span> element if the computed property hasNotifications returns true.

`v-bind:class` takes an object as its value : The keys of this object are class names and the values are Vue app properties that return a truthy or falsy value.

Similar to before with v-bind:style, you can also set the value of v-bind:class to a Vue app property that returns a class object instead of writing the object out in your HTML file.

### Class Arrays

```jsx
// index.html
<span v-bind:class="[{ unread: hasNotifications }, menuItemClass]">Notifications</span>

// app.js
const app = new Vue({
  data: { 
    notifications: [ ... ],
    menuItemClass: 'menu-item'
  },
  computed: {
    hasNotifications: function() {
      return notifications.length > 0;
    }
  }
});

// style.css
.menu-item {
  font-size: 12px;
}
 
.unread {
  background-color: blue;
}
```

The object at the beginning of the array will still conditionally add the unread class based on whether there are unread notifications. However, we now always add the class stored at menuItemClass, menu-item, to our “Notifications” element.

Using an array with v-bind:class is useful for adding non-conditional classes, like the menu-item class above, in addition to our conditional classes. We can also use this array to add multiple class objects like we did with our inline style objects earlier in the lesson.
