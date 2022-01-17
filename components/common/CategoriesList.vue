<template>
  <div class="container">
    <h2>Список категорий</h2>
    <div class="row">
      <div
        v-for="category in categories"
        :key="category.cSlug"
        :class="$style.block"
        class="col-md-3 col-sm-4 col-6 category"
      >
        <nuxt-link :to="`/category/${category.cSlug}`">
          <img :src="category.cImage" />
		  <p class="name">{{ category.cName }}</p>
        </nuxt-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    categories: {
      type: Array,
      default: () => []
    }
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

.wrapper {
  display: flex;
  flex-wrap: wrap;
  margin-left: auto;
  margin-right: auto;
  width: max-content;
  @include respond-to(medium-screens) {
	width:auto;
  }

}

h2{
  text-align: center;
  padding-bottom: 10px;
}

.block {
	
	a{
		text-decoration: none;
		@include respond-to(medium-screens){
			width: max-content;
			margin-left: auto;
			margin-right: auto;
		}
		
		img{
			width: 250px;
			height: 250px;
		}
		
		p{
			overflow: hidden;
			font-size: 18px;
			font-weight: 700;
			line-height: 22px;
			text-align: center;
			color: #363636;
		}
	}
	
}
</style>
