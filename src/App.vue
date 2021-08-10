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
    <ul>
      <li
        v-for="ingredient in expandedRecipe.ingredients"
        :key="ingredient.ingredient"
      >
        {{ingredient.amount}} {{ingredient.ingredient}}
      </li>
    </ul>

    <h3> Instructions </h3>
    <ol>
      <li
        v-for="instruction in expandedRecipe.instructions"
        :key="instruction"
      >
        {{instruction}}
      </li>
    </ol>

  </span>
</main>
</template>

<script> //

export default {
  data() {
    return {
      recipes: [
        {
          name: "Chickpeas with Rice & Peanuts",
          ingredients: [
            {
              ingredient: "chickpeas",
              amount: "160g"
            },
            {
              ingredient: "white rice",
              amount: "160g"
            },
            {
              ingredient: "peanuts",
              amount: "40g"
            },
            {
              ingredient: "iodized salt",
              amount: "0.1g"
            }
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
            {
              ingredient: "Müsli",
              amount: "60g"
            }
          ],
          instructions: [
            "Pour müsli into bowl."
          ]
        }
      ],
      expandedRecipe: null
    }
  },
  methods: {
    expandRecipe(recipe) {
      this.expandedRecipe = recipe
    }
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
  width: 100%;
  font-size: 1.2em;
}

.recipes li:nth-child(even) button {
  background-color: violet;
}
</style>
