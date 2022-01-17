<template>
  <tbody v-if="productsFromCart" >
    <tr
      v-for="product in productsFromCart"
      :key="product.productId"
      class="checkout_item"
    >
	  <td class="product-name">{{ product.meta.pName }} <strong>x{{product.qty}}</strong></td>
	  <td class="product-total">{{ (product.meta.pPrice * product.qty) | round }}</td>
    </tr>
  </tbody>

</template>

<script>
import CloseOrDeleteButton from '~~/components/common/input/CloseOrDeleteButton.vue'
import round from '~~/mixins/round'
import { mapActions } from 'vuex'
import debounce from 'lodash.debounce'
export default {
  components: {
    CloseOrDeleteButton
  },
  mixins: [round],
  props: {
    productsFromCart: {
      type: Array,
      default: () => []
    }
  },
  methods: {
      ...mapActions({
      setProductQuantity: 'cart/setProductQuantity',
      removeProduct: 'cart/removeProduct'
    }),
   onRemoveClickHandler (product) {
      this.removeProduct(product.productId)
    }
  }
}
</script>

<style lang="scss" module>
.input {
  height: 20px;
}
    .remove {
      top: -15px;
      position: absolute;
      left: -30px;
      z-index: 1;
    }
.wrapper {
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
  .product {
    position: relative;
    margin: 1em;
    display: flex;
    flex-direction: row;

    * {
      margin-right: 10px;
    }
    .pName {
      width: 150px;
    }
  }

  p {
    max-width: 270px;
    height: 35px;
  }
}
.image {
  width: 75px;
  height: 75px;
  object-fit: cover;
}

td{
	max-width: 50%;
	width: 50%;
	padding: 15px 10px;
	border: none;
	border-bottom: 2px solid #EFEFEF;
	font-weight: 400;
	font-size: 14px;
	line-height: 1.2;
}




</style>
