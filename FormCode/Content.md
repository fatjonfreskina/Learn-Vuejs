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