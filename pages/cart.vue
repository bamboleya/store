<template>
	<div class="flex">
		<div class="col-cart">
		  <table class="shop-table " id="cart-table" cellspacing="0">
			<thead>
					<tr>
						<th class="product-remove">&nbsp;</th>
						<th class="product-thumbnail">&nbsp;</th>
						<th class="product-name">Товар</th>
						<th class="product-price">Цена</th>
						<th class="product-quantity">Количество</th>
						<th class="product-subtotal">Сумма</th>
					</tr>
				</thead>
			  <ProductsList class="products" :products-from-cart="getProducts" />
		  </table>
		</div> 
		<div class="cart-totals-section">
			<div class="cart_totals calculated_shipping">
				<div class="cart-totals-inner">
					<h2>Сумма заказов</h2>
					<table cellspacing="0" class="shop_table shop_table_responsive">
						<tbody>
						<tr class="cart-subtotal">
							<th>Сумма</th>
							<td data-title="Сумма"><span class="woocommerce-Price-amount amount"><bdi>{{ getAmount | round }}<span class="woocommerce-Price-currencySymbol">грн</span></bdi></span></td>
						</tr>
						<tr class="woocommerce-shipping-totals shipping">
							<th>Доставка</th>
							<td data-title="Доставка">
									Новая почта: 50.00 грн
							</td>
						</tr>
						<tr class="order-total">
							<th>Итого</th>
							<td data-title="Итого"><span id="wcus-order-total"><strong><span class=" amount"><bdi>{{ getAmount+50 | round }}<span class="woocommerce-Price-currencySymbol">грн</span></bdi></span></strong> </span></td>
						</tr>


						</tbody>
					</table>

				<div class="wc-proceed-to-checkout">

				<a href="/checkout/" class="checkout-button button alt wc-forward">
				Оформить заказ</a>
				</div>

				</div><!--.cart-totals-inner-->
			</div>
		</div>
	</div>
</template>
<script>
import { mapGetters } from 'vuex'
import ProductsList from '~~/components/cart/ProductsList.vue'
import CloseOrDeleteButton from '~~/components/common/input/CloseOrDeleteButton.vue'
import round from '~~/mixins/round.js'
export default {
  components: {
    ProductsList,
    CloseOrDeleteButton
  },
  mixins: [round],
  data () {
    return {
      addedProduct: null,
      defaults: {
        addedProduct: null
      }
    }
  },

  computed: {
    ...mapGetters({
      getProductsInCart: 'cart/getProductsInCart'
    }),
    getAddedProduct () {
      const product = this.getProductsInCart.find(
        prod => prod.productId === this.addedProduct
      )
      if (product) {
        return [product]
      } else {
        return null
      }
    },
    getAmount () {
      let amount = 0
      if (this.getProductsInCart) {
        this.getProductsInCart.forEach(product => {
          amount +=
            parseFloat(product.meta.pPrice) *
            parseInt(product.qty)
        })
        return amount
      } else {
        return 0
      }
    },
    getProducts () {
      if (this.addedProduct) {
        return this.getProductsInCart.filter(
          prod => prod.productId !== this.addedProduct
        )
      } else {
        return this.getProductsInCart
      }
    }

  },

 
  methods: {
    async beforeOpen (event) {
      if (!event.params) {
        event.params = {}
      }
      if (event.params.addedProduct) {
        this.addedProduct = event.params.addedProduct
      } else {
        this.addedProduct = this.defaults.addedProduct
      }
    }
  }
}
</script>
<style lang="scss" scoped>
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

#cart-table tr{ @include respond-to(medium-screens) {
			display: block; 
			margin-bottom: 25px;
			padding-bottom: 25px;
			padding-left: 115px;
			min-height: 136px;
			position: relative;
			border-bottom: 1px solid rgba(129,129,129,.2);} 
	}
	
	.cart-totals-inner {
    padding: 25px;
    border: 3px solid #EFEFEF;
}

	.cart-totals-inner table {
		margin-bottom: 10px;
	}

	table tbody th, table tfoot th {
		border-bottom: 1px solid #E6E6E6;
		text-transform: none;
		font-size: 14px;
	}

	.shop_table tr td:last-child, .shop_table tr th:last-child {
		text-align: left;
	}
	.col-cart{
		width: 70%;
		@include respond-to(medium-screens) { width: 100%; }
	}
	
	.cart-totals-section{
		width: 30%;
		@include respond-to(medium-screens) { width: 100%; }
	}
	.flex{    
		display: flex;
		margin-top: 20px;
		width: 94%;
		margin-left: auto;
		margin-right: auto;
		@include respond-to(medium-screens) { display: block; }
	}
	
  .cart-list-header > th{
      padding: 0.5rem}
	  
	 
</style>
