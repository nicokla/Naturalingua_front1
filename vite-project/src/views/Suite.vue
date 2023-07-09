<template>
  <div class="liste">
		<h1 class="text-3xl mb-2">
			Purchase summary
		</h1>
		<div class="m-3">
			After payment, we will send your document <b>{{fileName}}</b> to study <b>{{language}}</b> to the email address <b>{{email}}</b> for {{price}} euros. If the document or the email address is incorrect, please come back to the previous page and choose the correct document and email address.
		</div>
    <stripe-checkout
      ref="checkoutRef"
      mode="payment"
      :pk="publishableKey"
      :lineItems="lineItems"
      :successUrl="successURL"
      :cancelUrl="cancelURL"
			:sessionId="sessionId"
      @loading="v => loading = v"
    />
    <button class="my-button" @click="submit">Checkout</button>
  </div>
</template>


<script>
// https://dashboard.stripe.com/settings/account?support_details=true
// https://vitejs.dev/guide/env-and-mode.html
// https://dashboard.stripe.com/test/dashboard
// https://vuestripe.com/stripe-checkout/one-time-payment

import { StripeCheckout } from '@vue-stripe/vue-stripe';
export default {
  components: {
    StripeCheckout,
  },
  data () {
    return {
			backend:'https://naturalingua.herokuapp.com', //'http://127.0.0.1:5000', // 'https://naturalingua.herokuapp.com'
			sessionId: '',
			publishableKey: 'pk_live_51KCkGtL309RW9KQT3X8xJbr279hnoVOInlYz1kNWOIzGM5DcBtH78r8BIPXQPKvMxEaerNCugl7pl3XvN79wK6vc00q4OIC5Cl', // import.meta.env.VITE_PUBLISHABLE_KEY_TEST,
      loading: false,
      lineItems: [
        {
          price: 'price_1KDuBdL309RW9KQTgwtkt4u8',
          quantity: 1,
        },
      ],
			fileName:this.$route.params.fileName,
			price:this.$route.params.price,
			email:this.$route.params.email,
			language:this.$route.params.language,
			docType:this.$route.params.docType,
			id:this.$route.params.id,
			channel:this.$route.params.channel
    };
  },
  methods: {
    async submit () {
			if(this.sessionId == ''){
				window.alert('There was a problem, please try again in a few seconds.')
			}
			else{
				window.alert('After clicking ok, you will be redirected to checkout within a few seconds.')
				this.$refs.checkoutRef.redirectToCheckout();
			}
    },
  },
	computed:{
		successURL(){return `${this.backend}/success/${this.email}`},
		cancelURL(){return `${this.backend}/cancel`},
	},
	async mounted() {
		let truc= await fetch(`${this.backend}/stripe_pay/${this.id}/${this.email}`)
		let trucjson= await truc.json()
		this.sessionId = trucjson.checkout_session_id
	}
};

</script>




<style scoped>
.liste{
	padding: 0.8em;
	display: flex;
	flex-direction: column;
	flex-wrap: nowrap;
	justify-content: flex-start;
	align-items: center;
	align-content: space-between;
}
.my-button{
  @apply bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mt-3
}
</style>
