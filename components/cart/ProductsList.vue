<template>

<tbody v-if="productsFromCart">
	<tr
      v-for="product in productsFromCart"
      :key="product.productId"
      class="cart_item"  
    >
      				
<template>
<td class="product-remove">
	<CloseOrDeleteButton
			  :class="$style.remove"
			  button-type="delete"
			  @click.native="onRemoveClickHandler(product)"
			/>
</td>

<td class="product-thumbnail">
<nuxt-link :to="`/product/${product.meta.pSlug}`">
          <img
            v-lazy="product.meta.images.imgL"
            :class="$style.image"
          />
        </nuxt-link>
</td>

<td class="product-name">
	<nuxt-link :class="$style.pName" :to="`/product/${product.meta.pSlug}`">
          <p>{{ product.meta.pName }}</p>
	</nuxt-link>
</td>

<td class="product-price" data-title="Цена">
<span class="amount"><bdi>{{ product.meta.pPrice }}<span class="woocommerce-Price-currencySymbol">грн</span></bdi></span>								
</td>

<td class="product-quantity" data-title="Количество">
<div class="quantity">
<input type="button" value="-" class="minus" @click="onQuantityChangeMinus(product)">
<input
	:value="product.qty"
	:class="$style.input"
	type="number"
	:min="1"
	:max="1000"
	:step="1"
	@input.prevent="onQuantityChangeHandler($event, product)"
	
/>
<input type="button" value="+" class="plus" @click="onQuantityChangePlus(product)">
</div>
</td>

<td class="product-subtotal" data-title="Сумма">
<span class="woocommerce-Price-amount amount"><bdi>{{ (product.meta.pPrice * product.qty) | round }}<span class="woocommerce-Price-currencySymbol">грн</span></bdi></span>								</td>
</template>
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
    },
    onQuantityChangeHandler: debounce(function onQuantityChangeHandler (e, product) {
      const qty = e.target.value
      this.setProductQuantity({ productId: product.productId, qty })
    }, 400),
	
	onQuantityChangePlus: debounce(function onQuantityChangePlus (product) {
		const qty = Number(product.qty)+1
      
      this.setProductQuantity({ productId: product.productId, qty })
    }, 400),
	onQuantityChangeMinus: debounce(function onQuantityChangeMinus (product) {
		let qty = Number(product.qty)
		if(qty >1){
			qty-=1
		}
      this.setProductQuantity({ productId: product.productId, qty })
    }, 400),
  }
}
</script>

<style lang="scss" module>
	$small: 320px;
$large: 1024px;

@mixin respond-to($media) {
  @if $media == handhelds {
    @media only screen and (max-width: $small) { @content; }
  }
  @else if $media == medium-screens {
    @media only screen and (min-width: $small + 1) and (max-width: $large - 1) { @content; }
  }
  @else if $media == wide-screens {
    @media only screen and (min-width: $large) { @content; }
  }
}

.input {
  height: 20px;
}
    .remove {
      top: -15px;
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


table{
	margin-bottom: 15px;
    width: 100%;
    border-spacing: 0;
    border-collapse: collapse;
    font-size: 14px;
    line-height: 1.4;
	@include respond-to(medium-screens) { display: block; }
	
	thead {
		@include respond-to(medium-screens) { display: none;}
		
		th{
			padding: 15px 10px;
			border: none;
			border-bottom: 2px solid #EFEFEF;
			vertical-align: middle;
			text-align: left;
			text-transform: uppercase;
			font-weight: 600;
			font-size: 16px;
			line-height: 1.2;
		}
	}
	
	tbody{
		@include respond-to(medium-screens) { display: block; }
	}
}


table td {
    padding: 15px 12px;
    border: none;
    border-bottom: 1px solid #E6E6E6;
    text-align: left;
    font-weight: inherit;
}

td.product-thumbnail img {
    min-width: 80px;
    max-width: 80px;
	
	@include respond-to(medium-screens){
		position: absolute;
		top: 0;
		left: 0;
		overflow: hidden;
		margin-bottom: 0;
		padding-bottom: 0;
		max-height: 115px;
		border-bottom: none;
		display: flex;
		align-items: center;
		flex-direction: row;
		flex-wrap: wrap;
	}
}

th.product-remove {
    width: 40px;
}

th.product-thumbnail {
    width: 10px;
}

.product-name {
    text-align: left;
}

input[type=number], input[type=number]:hover {
		width: 30px;
		height: 38px;
		border-right: none;
		border-left: none;
		margin: 0;
		-webkit-appearance: none;
		-moz-appearance: none;
		appearance: none;
		border-top-style: solid;
		display: inline-block;
		text-align: center;
		border: 1px solid #e6e6e6;
		@include respond-to(medium-screens){
			height: 30px;
		}
		&:focus {
			outline: -webkit-focus-ring-color auto 0px;
		}
	}
	
	input[type=button] {
		padding: 0 5px;
		min-width: 25px;
		height: 42px;
		margin: 0px -5px;
		border: 1px solid rgba(129,129,129,.2);
		background: 0 0;
		box-shadow: none;
		@include respond-to(medium-screens){
			height: 34px;
		}
		&:focus {
			outline: -webkit-focus-ring-color auto 0px;
		}
		
		&:hover{
		background: #fcc000;
		}
	}

input[type=number]::-webkit-inner-spin-button, 
input[type=number]::-webkit-outer-spin-button { 
  -webkit-appearance: none; 
  margin: 0; 
}


td.product-remove {
    padding: 0;
    text-align: center;
}

td.product-remove a {
    width: 30px;
    height: 30px;
    vertical-align: middle;
    font-size: 0;
}


	

</style>
