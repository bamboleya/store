<template>
  <div v-if="product">
    <client-only>
      <button
        v-if="!isProductAdded"
        :class="$style.buy"
        @click.prevent="buyClickHandler"
      >
        Buy
      </button>
      <a
        v-else
        :class="$style.added"
        href="/cart"
        @click.prevent="addedClickHandler"
      >
        Already in cart
      </a>
    </client-only>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'

export default {
  props: {
    product: {
      type: Object,
      required: true
    }
  },
  computed: {
    ...mapGetters({
      getProductsInCart: 'cart/getProductsInCart'
    }),
    isProductAdded () {
      return this.getProductsInCart.find(p => p.productId === this.product.id)
    }
  },
  methods: {
    ...mapActions({
      addProduct: 'cart/addProduct'
    }),
    buyClickHandler () {
      this.addProduct(this.product.id)
      //this.$modal.show('customer-cart', { addedProduct: this.product.id })
    },
    addedClickHandler () {
      //this.$modal.show('customer-cart', { addedProduct: this.product.id })
    }
  }
}
</script>

<style lang="scss" module>
.buy {
  background-color: $basic-bg-color; /* Green */
  border: none;
  border-radius: 45px;
  width: 100%;
  color: #fff;
  padding: 10px 0px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;

  &:hover {
    cursor: pointer;
  }
  
  &:focus{
	outline: 0;
  }
}

.added {
  background-color: $basic-bg-color; /* Green */
  border: none;
  border-radius: 45px;
  width: 100%;
  color: #fff;
  padding: 10px 0px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;

  &:hover {
    cursor: pointer;
  }
}
</style>
