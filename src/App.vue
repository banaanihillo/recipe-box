<template>
<main id="app">
  <h1> Recipe Box </h1>

  <ul class="recipes">
    <li v-for="recipe in recipes" :key="recipe.name">
      <button @click="expandRecipe(recipe)">
        {{recipe.name}}
      </button>
    </li>
  </ul>

  <span v-if="expandedRecipe">
    <h2> {{expandedRecipe.name}} </h2>

    <h3> Ingredients </h3>
    <section>
      <ul>
        <li
          v-for="ingredient in expandedRecipe.ingredients"
          :key="ingredient"
        >
          {{ingredient}}
        </li>
      </ul>

      <button @click="editingIngredients=!editingIngredients">
        Edit ingredients
      </button>
      <EditRecipe
        v-if="editingIngredients"
        formLabel="Ingredients"
        @edit-recipe="editRecipe"
        :recipeName="expandedRecipe.name"
      />
    </section>

    <h3> Instructions </h3>
    <section>
      <ol>
        <li
          v-for="instruction in expandedRecipe.instructions"
          :key="instruction"
        >
          {{instruction}}
        </li>
      </ol>

      <button @click="editingInstructions=!editingInstructions">
        Edit instructions
      </button>
      <EditRecipe
        v-if="editingInstructions"
        formLabel="Instructions"
        @edit-recipe="editRecipe"
        :recipeName="expandedRecipe.name"
      />
    </section>

    <section class="delete">
      <button @click="deleteRecipe(expandedRecipe)">
        Delete
      </button>
    </section>
  </span>
  <hr />

  <h2> Add new recipe </h2>
  <span v-if="formOpen">
    <button @click="formOpen=false">
      Close form
    </button>
    <AddRecipe @add-recipe="addRecipe" />
  </span>
  <span v-else>
    <button @click="formOpen=true">
      Open form
    </button>
  </span>
</main>
</template>

<script>
import AddRecipe from "./components/AddRecipe.vue"
import EditRecipe from "./components/EditRecipe.vue"

export default {
  components: {
    AddRecipe,
    EditRecipe
  },
  data() {
    return {
      recipes: [],
      expandedRecipe: null,
      formOpen: false,
      editingIngredients: false,
      editingInstructions: false
    }
  },
  methods: {
    expandRecipe(recipe) {
      this.expandedRecipe = recipe
      this.collapseForms()
    },
    addRecipe(input) {
      this.recipes = [
        ...this.recipes,
        input
      ]
      this.saveRecipes()
      this.formOpen = false
    },
    editRecipe(input) {
      const recipeToEdit = this.recipes.find((recipe) => {
        return recipe.name === input.name
      })
      const replacedRecipe = {
        ...recipeToEdit,
        [input.type]: input.steps
      }

      this.expandedRecipe = replacedRecipe 
      this.recipes = this.recipes.map((recipe) => {
        return (
          recipe.name === input.name
            ? replacedRecipe
            : recipe
        )
      })
      this.saveRecipes()
      this.collapseForms()
    },
    collapseForms() {
      this.editingIngredients = false
      this.editingInstructions = false
    },
    initializeRecipes() {
      const existingRecipes = localStorage.getItem("recipes")
      if (existingRecipes) {
        return JSON.parse(existingRecipes)
      } else {
        return [
        {
          name: "Chickpeas with Rice & Peanuts",
          ingredients: [
            "160g chickpeas, dried",
            "160g white rice",
            "40g peanuts",
            "0.1g iodized salt"
          ],
          instructions: [
            "Soak chickpeas in water for 8+ hours, or overnight.",
            "Boil fresh new batch of water and add salt.",
            "Rinse chickpeas well and discard the soak water.",
            "Once boiling, reduce heat to a simmer and add chickpeas.",
            "Let simmer for ~30 minutes under lid.",
            "Add rice, and seasoning & herbs if desired. Stir well.",
            "Let simmer for ~15 minutes under lid. Stir occasionally.",
            "Turn the heat off and let the pot sit for 5-10 minutes.",
            "Discard any leftover water if necessary."
          ]
        },
        {
          name: "Müsli with Seeds and Dried Berries",
          ingredients: [
            "60g müsli"
          ],
          instructions: [
            "Pour müsli into bowl."
          ]
        }
        ]
      }
    },
    saveRecipes() {
      localStorage.setItem(
        "recipes",
        JSON.stringify(this.recipes)
      )
    },
    deleteRecipe(selectedRecipe) {
      const recipeToDelete = this.recipes.find((recipe) => {
        return recipe.name === selectedRecipe.name
      })
      const filteredRecipes = this.recipes.filter((recipe) => {
        return recipe.name !== recipeToDelete.name
      })
      this.recipes = filteredRecipes
      this.expandedRecipe = null
      this.saveRecipes()
    }
  },
  created() {
    this.recipes = this.initializeRecipes()
  }
}
</script>

<style>
body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  background-color: black;
  text-align: center;
  color: magenta;
  width: 50%;
  max-width: 600px;
  margin: 0 auto;
}

ul,
ol {
  padding-left: 0;
}

li {
  padding: 0.2em;
}

button {
  background-color: plum;
}

.recipes button {
  width: 100%;
  font-size: 1.2em;
}

.recipes li:nth-child(even) button {
  background-color: violet;
}

button:focus,
button:hover {
  border: 2px solid darkmagenta;
}

input,
textarea {
  background-color: plum;
  margin-bottom: 1em;
}

.delete {
  border: 2px solid tomato;
  padding: 1em;
  margin: 1em;
}

.delete button {
  background-color: salmon;
}
</style>
