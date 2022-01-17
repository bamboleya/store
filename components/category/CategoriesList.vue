<template>
  <div class="col-sm-2 d-none d-md-block">
    <h4>Категории</h4>
    <ul>
      <li
        v-for="category in categories"
        :key="category.cSlug"
      >
        <nuxt-link :to="`/category/${category.cSlug}`">
          <p>{{ category.cName }}</p>
        </nuxt-link>
      </li>
    </ul>
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

ul{
  list-style:none;
  padding-left: 10px;

	a{
		text-decoration: none;
		@include respond-to(medium-screens){
			width: max-content;
			margin-left: auto;
			margin-right: auto;
		}
				
		p{
			overflow: hidden;
			font-size: 18px;
			font-weight: 700;
			line-height: 22px;
			color: #363636;
		}
	}
}
</style>
