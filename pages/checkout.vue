<template>

<div class="main">
	<div class="col-checkout" id="form-check">
		<div class="review-form">
			<h3>Оплата и доставка</h3>
			<div class="form-wrap">
				<p class="first-label">
					<label for="name">Имя:</label>
					<input id="name" v-model="name">
				</p>
				<p class="last_name second-label">
					<label for="last_name">Фамилия:</label>
					<input id="last_name" v-model="last_name">
				</p>
				<p class="tel first-label">
					<label for="tel">Телефон:</label>
					<input id="tel" v-model="tel">
				</p>
				<p class="mail second-label">
					<label for="mail">E-mail:</label>
					<input id="mail" v-model="mail"> 
				</p>
			</div>
		</div>
		<div class="shipping" v-if="check===off">
			<h3>Укажите отделение доставки</h3>
			<p class="area">
				<label for="area">Область</label>
				<input id="area" v-model="area">
			</p>
			<p class="city">
				<label for="city">Город</label>
				<input id="city" v-model="city">
			</p>
			<p class="warehouse">
				<label for="warehouse">Отделение</label>
				<input id="warehouse" v-model="warehouse">
			</p>
		</div>
		<div class="address_shipping">
			<div class="address_shiping_form" v-if="check===on">
				<h3>Адресная доставка</h3>
				<p class="city_address">
					<label for="city_address">Населенный пункт</label>
					<input id="city_address" v-model="city_address">
				</p>
				<p class="street">
					<label for="street">Улица</label>
					<input id="street" v-model="street">
				</p>
				<p class="building">
					<label for="building">Дом</label>
					<input id="building" v-model="building">
				</p>
				<p class="apart">
					<label for="apart">Квартира</label>
					<input id="apart" v-model="apart">
				</p>
			</div>
			<label class="shipping-checkbox" for="address_active">
				<input id="address_active" type="checkbox" v-model="check" v-bind:true-value="on" v-bind:false-value="off" name="address_active">
				 Нужна адресная доставка 
			</label>
	
		</div>
		<div class="additions">
			<h3>Детали</h3>
			<p class="comment">
				<textarea name="order_comments" class="input-text" id="order_comments" placeholder="" v-model="comment" rows="2" cols="5"></textarea>
			</p>
		</div>
		<div class="call-me">
			<label class="call-checkbox" for="call_me_baby">
					<input id="call_me_baby" type="checkbox" v-model="call" v-bind:true-value="on" v-bind:false-value="off" name="call_me_baby">
					Не перезванивайте мне 
			</label>
		</div>
	</div>
	<div class="col-checkout">
		<div class="checkout-order">
			<h3 id="order-heading">Ваш заказ</h3>
			<div id="order-review">
				<div class="table-wrapper">
					<table class="checkout-table">
						<thead>
							<tr>
								<th class="product-name">
									Товар
								</th>
								<th class="product-total">
									Сумма
								</th>
							</tr>
						</thead>
						<ProductsList class="products" :products-from-cart="getProducts" /> 
						<tfoot>
							<tr class="cart-subtotal">
								<th>Сумма</th>
								<td>
									<span class="amount">{{ getAmount | round }} грн</span>
								</td>
							</tr>			
							<tr class="shipping-table">
								<th>Доставка</th>
								<td data-title="Доставка">
									Новая почта: 50.00 грн	
								</td>
							</tr>		
							<tr class="order-total">
								<th>Итого</th>
								<td><span>{{ getAmount+50 | round }} грн</span></td>
							</tr>
						</tfoot>
					</table>
				</div>
				<h3>Варианты оплаты</h3>
				<div id="payment">
					<ul class="payment_methods">
						<li class="payment_method_cod">
							<input id="payment_method_cod" type="radio" class="input-radio" name="payment_method" value="cod" checked v-model="payment" data-order_button_text="">
							<label for="payment_method_cod">Оплата при доставке </label>
							<div class="payment_box payment_method_cod" v-show="payment === 'cod'" style="" >
								<p>Оплата наличными при доставке заказа.</p>
							</div>
						</li>
						<li class="payment_method_transfer">
							<input id="payment_method_transfer" type="radio" class="input-radio" name="payment_method" value="transfer" v-model="payment" data-order_button_text="">
							<label for="payment_method_liqpay-webplus">Денежный перевод</label>
							<div class="payment_box payment_method_transfer" v-show="payment === 'transfer'" style="">
								<p>Платежный сервис, который позволяет совершать моментальные платежи в интернете и платежных карт Visa, MasterCard во всём мире.</p>
							</div>
						</li>
					</ul>
					<div class="form-row place-order">
						<div class="button alt" name="checkout_place_order" @click.prevent="sendTelegram()" id="place_order" value="Подтвердить заказ" data-value="Подтвердить заказ">Подтвердить заказ</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</div>


</template>
<script>
import { mapGetters } from 'vuex'
import ProductsList from '~~/components/checkout/ProductsList.vue'
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
		on: true,
        off:false,
        check: false,
		name:'',
		last_name:'',
		tel:'',
		mail:'',
		area:'',
		sity:'',
		warehouse:'',
		city_address:'',
		street:'',
		apart:'',
		order_comments:'',
		call:false,
		comment:'',
		payment:'cod',
      defaults: {
        addedProduct: null,
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
    },	
	
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
    },
	sendTelegram(){
		const TOKEN = ''; // токен от BotFather
		const CHAT_ID = ''; // chat_id для телеграм

		var url = 'https://api.telegram.org/bot' + TOKEN + '/sendMessage'; // токен бота
		var product_list='';
		var sum = 0;
		if(this.payment == 'cod'){
			var pay="Оплата при доставке";
		}else{
			var pay="Денежный перевод";
		}
		if (this.getProductsInCart) {
        this.getProductsInCart.forEach(product => {
          product_list += product.meta.pName + '\t*x*'+ parseInt(product.qty)+'\n';
			sum+=parseFloat(product.meta.pPrice) * parseInt(product.qty);
        });
		}
		if(!this.check){
			var text_msg = '*Имя:* ' + this.name +'\n*Фамилия:* '+this.last_name+ '\n*Телефон:* '+this.tel+'\n*E-mail:* '+this.mail+'\n*Доставка на отделелние*\nОбласть: '+this.area+'\nГород: '+this.city+'\nОтделение: '+this.warehouse+'\nКомментарий: '+ this.comment+'\nЗвонить: '+this.call+"\n*Товары*:\n"+product_list+'\n*Итого:* '+sum+' .грн';
		}else{
			var text_msg = "не";
		}
		var body = JSON.stringify({ // склеиваем объект в JSON строку
			chat_id: CHAT_ID,
			parse_mode: 'Markdown', // разметка сообщений вкл (чтобы использовать *жирный текст*)
			text: text_msg
		});
		var xhr = new XMLHttpRequest(); // инициализируем AJAX запрос
		xhr.open('POST', url, true); // отправляем наше сообщение методом POST на сервак телеги
		xhr.setRequestHeader('Content-type', 'application/json; charset=utf-8'); // на всякий случай, оповестим телеграм, что отправили JSON
		xhr.send(body);
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

.container{
	width: 100%;
    padding-right: 15px;
    padding-left: 15px;
    margin-right: auto;
    margin-left: auto;
}

.main{
	display: flex;
	margin-top: 15px;
	@include respond-to(medium-screens) { display: block;}
}

.col-checkout{
	width: 50%;
	padding-right: 30px;
    padding-left: 30px;
    margin-right: auto;
    margin-left: auto;
	@include respond-to(medium-screens) {
		width: 94%;
		padding-right: 0;
		padding-left: 0;
		margin-right: auto;
		margin-left: auto;
	}
	
	 h3{
		font-weight: 600;
		font-size: 22px;
	}
}

.checkout-order{
	position: relative;
    margin-bottom: 40px;
	margin-top: 30px;
    padding: 30px;
	@include respond-to(medium-screens) { padding: 0px 30px 0 14px; }
    background-color: #F8F8F8;
	
	&:before, &:after{
		content: "";
		position: absolute;
		left: 0;
		width: 100%;
		height: 10px;
		background-color: transparent;
		background-image: radial-gradient(farthest-side,rgba(0,0,0,0) 6px,#f8f8f8 0);
		background-size: 15px 15px;
	}
	
	&:before{
		top: -10px;
		background-position: -3px -5px,0 0;
	}
	
	&:after{
		bottom: -10px;
		background-position: -3px 2px,0 0;
	}
}

#order-heading{
	text-align: center;
}

.table-wrapper{
	width: 96%;
	overflow-x: auto;
    margin-bottom: 20px;
    padding: 5px 15px;
    background-color: #FFF;
    box-shadow: 1px 1px 2px #0000000d;
}

.checkout-table{
	width: 100%;
	
	th,td{
		max-width: 50%;
		width: 50%;
		padding: 15px 10px;
		border: none;
		border-bottom: 2px solid #EFEFEF;
		font-weight: 600;
		font-size: 16px;
		line-height: 1.2;
	}
	
}
.first-label{float:left;}
	.second-label{float:right;}

.review-form, .shipping,.address_shiping_form, .additions{
	margin: 0;
    padding: 0;
    border: 0;
    vertical-align: baseline;
    font: inherit;
    font-size: 100%;
	
	h3{
		display: inline-block;
		width: 100%;
	}
	
	p{
		display: flex;
		align-items: center;
		margin-top: 14px;
		flex-direction: row;
		border-bottom: 2px solid rgba(129,129,129,.2);
		transition: border-color .4s ease;
		overflow: visible;

		label{
			flex: 0 0 auto;
			margin-bottom: 0;
			margin-right: 15px;
			font-weight: 600;
			line-height: 22px;
		}
		
		input{
			padding: 0 2px;
			border-top-style: none;
			border-right-style: none;
			border-left-style: none;
			border-width: 1px;
			max-width: 100%;
			width: 100%;
			height: 30px;
			border: 0px;
			border-radius: 0;
			background-color: transparent;
			box-shadow: none;
			vertical-align: middle;
			font-size: 14px;
			transition: border-color .5s ease;
			
			&:focus{
				outline:-webkit-focus-ring-color auto 0px;
			}
		}
		
		textarea{
			padding-top: 12px;
			padding-bottom: 12px;
			min-height: 80px;
			padding: 0 2px;
			border: 2px solid #e6e6e6;;
			width: 100%;
			
			&:focus{
				outline: 0;
				border-color: rgba(129,129,129,.3);
				transition: border-color .4s ease;
			}
		}
	}
}

.additions p{border:0;}

.payment_methods{
	margin-bottom: 20px;
	line-height: 1.4;
	list-style: none;
	margin: 0;
    padding: 0;
    border: 0;
    vertical-align: baseline;
    font-size: 100%;
	
	.payment_box{
		position: relative;
		margin-top: 15px;
		padding: 15px;
		background-color: #FFF;
		box-shadow: 1px 1px 2px #0000000d;
		
		p{
			font-size: 15px;
			color: #000000;
		}
	}
	
	li {
		margin-bottom: 15px;
	}
}

.review-form p{
	@include respond-to(medium-screens) { width: 100%;}
    width: 48%;
	
}

.place-order{
	margin-top: 20px;
    padding-top: 20px;
    border-top: 1px solid #81818133;
	width: 100%;
    line-height: 20px;
	
	
	button{
		border-radius: 5px;
		width: 100%;
		font-size: 14px;
		padding: 20px 0;
		font-weight: 600;
	}
}

.shipping-checkbox{
	display: inline-block;
}

.address_shipping,.call-me{
	margin-top: 15px;
}


</style>