<template>
  <form @submit.prevent="editRecipe()">
    <label :for="`${formLabel}-input`"> {{formLabel}} </label>
    <br />
    <textarea
      :id="`${formLabel}-input`"
      rows="5"
      cols="40"
      placeholder="Steps separated by line breaks"
      v-model="steps"
      required
    ></textarea>
    <br />
    <button type="submit">
      Submit
    </button>
  </form>
</template>

<script>
export default {
  props: {
    formLabel: String,
    recipeName: String
  },
  data() {
    return {
      steps: ""
    }
  },
  methods: {
    editRecipe() {
      this.$emit(
        "edit-recipe",
        {
          name: this.recipeName,
          steps: this.steps.split("\n"),
          type: this.formLabel.toLowerCase()
        }
      )
    }
  }
}
</script>

<style>
form {
  padding: 1em;
}
</style>
