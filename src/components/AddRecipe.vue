<template>
  <form @submit.prevent="addRecipe()">
    <label for="name-input"> Name </label>
    <input
      id="name-input"
      type="text"
      v-model="input.name"
      required
    />

    <label for="ingredients-input"> Ingredients </label>
    <textarea
      id="ingredients-input"
      rows="5"
      cols="30"
      placeholder="Ingredients separated by line breaks"
      v-model="input.ingredients"
      required
    ></textarea>

    <label for="instructions-input"> Instructions </label>
    <textarea
      id="instructions-input"
      rows="5"
      cols="40"
      placeholder="Instructions separated by line breaks"
      v-model="input.instructions"
      required
    ></textarea>

    <button type="submit">
      Submit
    </button>
  </form>  
</template>

<script>
export default {
  data() {
    return {
      input: {
        name: "",
        ingredients: "",
        instructions: ""
      }
    }
  },
  methods: {
    addRecipe() {
      const ingredients = this.input.ingredients.split("\n")
      const instructions = this.input.instructions.split("\n")

      const input = {
        name: this.input.name,
        ingredients,
        instructions
      }
      this.$emit(
        "add-recipe",
        input
      )
    }
  }
}
</script>

<style scoped>
input,
textarea {
  background-color: plum;
  margin-bottom: 1em;
}

textarea {
  width: 100%;
}

input[type="text"] {
  width: 20em;
}

form {
  display: flex;
  flex-flow: column nowrap;
  align-items: center;
  padding: 1em;
}
</style>
