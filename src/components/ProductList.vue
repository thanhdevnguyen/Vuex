<template>
  <div>
    <h1>Product List</h1>
    <img
      v-if="loading"
      src="https://i.imgur.com/JfPpwOA.gif"
    >
    <ul v-else>
      <li v-for="product in products">
        {{ product.title }} - {{ product.price | currency }} - {{ product.inventory }}
        <button
          :disabled="!productIsInstock(product)"
          @click="addProductToCart(product)">Add to cart</button>
      </li>
    </ul>
  </div>
</template>

<script>

export default {
  name: "ProductList",

  data () {
    return {
      loading: false
    }
  },

  computed: {
    products () {
      return this.$store.getters.availableProducts
    },

    productIsInstock () {
      return this.$store.getters.productIsInStock
    }
  },

  methods: {
    addProductToCart(product) {
      this.$store.dispatch('addProductToCart', product)
    }
  },

  created () {
    this.loading = true;
    this.$store.dispatch('fetchProducts').then(() => {
      this.loading = false;
    })
  }
}
</script>

<style scoped>

</style>
