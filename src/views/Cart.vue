<template>
  <div class="mt-5">
  <div class="container">
    <div class="row">
      <div class="col-8">
        <h1 class="border-bottom mb-2">Cart</h1>
        <h6 v-show="!cartItems.length">Your cart is empty</h6>
        <cart-items
        v-for="item in cartItems"
        :key="item.id"
        :cartItem='item'
        @updateAmount='updateAmount'
        />
      </div>
      <div class="col-4">
        <div class="row p-3 border-start rounded overflow-hidden flex-md-row mb-4 shadow-sm">
          <h2>Cart summary</h2>
          <h6>Number of items: {{ numberofItems }}</h6>
          <h6>Total price: {{ new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR' }).format(totalPrice) }}</h6>
          <button @click.prevent='checkout' class="btn btn-primary mt-5">Checkout</button>
        </div>
      </div>
    </div>

  </div>
  </div>
</template>

<script>
import Swal from 'sweetalert2'
import CartItems from '../components/CartItems'
export default {
  name: 'Cart',
  components: {
    CartItems
  },
  computed: {
    cartItems () {
      return this.$store.state.cartItems
    },
    numberofItems () {
      return this.cartItems.length
    },
    totalPrice () {
      let total = 0
      this.cartItems.forEach(item => {
        total += (item.amount * item.Product.price)
      })
      return total
    }
  },
  created () {
    this.$store.dispatch('getCartItems')
  },
  methods: {
    updateAmount (payload) {
      this.$store.dispatch('updateAmount', payload)
    },
    checkout () {
      let safe = true
      console.log(this.cartItems)
      this.cartItems.forEach(item => {
        console.log(item.amount)
        if (!item.amount) {
          safe = false
        }
      })
      // console.log(safe)
      if (!safe) {
        Swal.fire({
          icon: 'error',
          title: 'Oops',
          text: 'Please insert amount',
          showConfirmButton: false,
          timer: 1000,
          timerProgressBar: true,
          toast: true,
          position: 'top-right'
        })
      } else {
        this.$store.dispatch('checkout')
      }
    }
  }

}
</script>

<style>

</style>
