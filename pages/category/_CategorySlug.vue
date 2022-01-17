<template>
  <div class="container">
    <div class="row">
      <div :class="$style.page">
        <h1>{{ category.cName }}</h1>
        <div class="row">
        <CategoriesList :categories="categories" />
        <div :class="$style.productList" class="col-sm-10">
          <div
            v-for="product in category.products"
            :key="product.id"
            class="col-lg-4 col-md-4 col-sm-6 col-xs-12"
          >
            <ProductBrief :product="product" />
          </div>
        </div>
        <p>{{ category.cDesc }}</p>
      </div>  
      </div>
    </div>
  </div>

</template>

<script>
import ProductBrief from '~~/components/category/ProductBrief'
import CategoriesList from '~~/components/category/CategoriesList'
import { mapState } from 'vuex'
export default {
  components: {
    ProductBrief,
    CategoriesList
  },
  async asyncData ({ app, params, route, error }) {
    try {
      await app.store.dispatch('getCurrentCategory', { route }),
      await app.store.dispatch('getCategoriesList')
    } catch (err) {
      console.log(err)
      return error({
        statusCode: 404,
        message: 'Категория не найдена или сервер не доступен'
      })
    }
  },
  computed: {
    ...mapState({
      category: 'currentCategory',
      categories: 'categoriesList'
    })
  },
  head () {
    return {
      title: this.category.cTitle,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.category.cMetaDescription
        }
      ]
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

.page {
  @include globalWrapper;

  h1{
    text-align: center;
    padding: 10px 0;
  }
}
.productList {
	  display: flex;
	  flex-wrap: wrap;
	  justify-content: space-between;
  
	&>div{
	  a{
		@include respond-to(medium-screens){margin-left: auto; margin-right: auto;}
	  }
  }
}
</style>
