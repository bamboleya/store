<template>
  <div :class="$style.wrapper">
    <nuxt-link :to="`/product/${product.pSlug}`">
      <img
        v-lazy="product.image.imgL"
        :class="$style.image"
      />
	   <p class="name">{{ product.pName }}</p>
    </nuxt-link>
    <p class="price">{{ product.pPrice }}<span class="currency"> грн.</span></p>
    <BuyButton :product="product" />
  </div>
</template>

<script>
import BuyButton from '~~/components/common/BuyButton'
export default {
  components: {
    BuyButton
  },
  props: {
    product: {
      type: Object,
      default: () => {}
    }
  }
}
</script>

<style lang="scss" module>
.wrapper {
  display: flex;
  flex-direction: column;
  margin-bottom: 1em;
   margin-right: auto;
	  margin-left: auto;
	  border: 1px solid #e3e3e3;
	  margin-bottom: 20px;
	  overflow: auto;
	  background: #fff;
	  padding: 20px;

  
  p {
    max-width: 270px;
  }
  
  a{
	text-decoration: none;
  width: 230px;
	
	p{
		overflow: hidden;
		font-size: 14px;
		min-height: 32px;
		font-weight: 700;
		line-height: 16px;
		text-align: center;
		color: #363636;
	}
  }
}

button:focus{
	outline: 0;
}
	
.image {
  width: 100%;
  height: 230px;
    object-fit: cover;
}
</style>
