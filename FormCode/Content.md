# In this lesson, we will:

- Bind all of the form fields to our Vue data
- Implement the functionality to reset and submit our form and clean up our form data
- Implement form validation to ensure users are using our form as expected

## Examples

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

## Checkboxes

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

## Boolean checkbox

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

## Form event handlers

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

## Form event modifiers

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

## Input modifiers

We can use modifiers to make our form fields even more versatile.

Vue offers the following three modifiers for v-model:

- .number — automatically converts the value in the form field to a number (to save the appropriate data type and not have to convert it later)
- .trim — removes whitespace from the beginning and ends of the form field value
- .lazy — only updates data values when change events are triggered (often when a user moves away from the form field rather than after every keystroke)

## Form Validation

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
- If the form is not valid, formIsValid will return false and the button will be disabled

