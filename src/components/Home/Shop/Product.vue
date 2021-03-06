<script setup lang="ts">

import { useCartStore } from '../../../store/Cart.js'
import { computed } from 'vue';

// const productProps = defineProps<{ product: string }>()
const productProps = defineProps<{ product: {
  id: BigInteger,
  name: String,
  marketPrice: BigInt,
  price: BigInteger,
  shortdesc: String,
  url: string
} }>()

const cartStore = useCartStore();

let cart = computed(() => {
    return cartStore.$state.cart
});

var itemAlreadyInCart = computed(() => {
  let inCart = false;

  cart.value.forEach((item: any) => {
    if(item.id == productProps.product.id){
      inCart = true;
    }
  });

  return inCart;
})

function addToCart(item: any) {
  if(!itemAlreadyInCart.value) {
      addToCart: cartStore.addToCart(item)
  } else {
      alert(`${item.name} already exists in your cart`)
      console.log(this.$toast)
  }
}


</script>

<template>
  <div class="col-md-3 col-sm-6">
    <div class="single-product">
      <!-- <div class="product-grid">
        <div class="product-image">
          <a href="#">
            <img
              class="pic-1"
              src="https://www.w3schools.com/bootstrap4/img_avatar4.png"
            />
            <img
              class="pic-2"
              src="https://www.w3schools.com/bootstrap4/img_avatar3.png"
            />
          </a>
          <span class="product-new-label">Sale</span>
          <span class="product-discount-label">20%</span>
        </div>
        <div class="product-content">
          <h3 class="title">
            <a href="#">{{ product.name }}</a>
          </h3>
          <div class="price">
            ${{ product.price }}
            <span>${{ product.marketPrice }}</span>
          </div>
          <button @click="addToCart(product)" class="btn btn-outline-secondary">
              Cart
          </button>
        </div>
      </div> -->
      <div class="card mb-4">
          <img :src="product.url" class="card-img-top" alt="...">
          <div class="card-body">
          <h5 class="card-title">
            <!-- <router-link :to="{ name: '/product/', params: { productId: product.price }}"> -->
            {{product.name}}
            <!-- </router-link> -->
          </h5>
          <p class="card-text">
              ${{product.price}}
              <br/>
              <small>
              {{product.shortdesc}}
              </small>
          </p>
          <button @click="addToCart(product)" class="btn btn-primary btn-block" :disabled="itemAlreadyInCart">{{itemAlreadyInCart? "Added" : "Add to Cart"}}</button>
          </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.product-grid {
  font-family: Raleway, sans-serif;
  text-align: center;
  padding: 0 0 72px;
  border: 1px solid rgba(0, 0, 0, 0.1);
  overflow: hidden;
  position: relative;
  z-index: 1;
}
.product-grid .product-image {
  position: relative;
  transition: all 0.3s ease 0s;
}
.product-grid .product-image a {
  display: block;
}
.product-grid .product-image img {
  width: 100%;
  height: auto;
}
.product-grid .pic-1 {
  opacity: 1;
  transition: all 0.3s ease-out 0s;
}
.product-grid:hover .pic-1 {
  opacity: 1;
}
.product-grid .pic-2 {
  opacity: 0;
  position: absolute;
  top: 0;
  left: 0;
  transition: all 0.3s ease-out 0s;
}
.product-grid:hover .pic-2 {
  opacity: 1;
}
.product-grid .social {
  width: 150px;
  padding: 0;
  margin: 0;
  list-style: none;
  opacity: 0;
  transform: translateY(-50%) translateX(-50%);
  position: absolute;
  top: 60%;
  left: 50%;
  z-index: 1;
  transition: all 0.3s ease 0s;
}
.product-grid:hover .social {
  opacity: 1;
  top: 50%;
}
.product-grid .social li {
  display: inline-block;
}
.product-grid .social li a {
  color: #fff;
  background-color: #333;
  font-size: 16px;
  line-height: 40px;
  text-align: center;
  height: 40px;
  width: 40px;
  margin: 0 2px;
  display: block;
  position: relative;
  transition: all 0.3s ease-in-out;
}
.product-grid .social li a:hover {
  color: #fff;
  background-color: #ef5777;
}
.product-grid .social li a:after,
.product-grid .social li a:before {
  content: attr(data-tip);
  color: #fff;
  background-color: #000;
  font-size: 12px;
  letter-spacing: 1px;
  line-height: 20px;
  padding: 1px 5px;
  white-space: nowrap;
  opacity: 0;
  transform: translateX(-50%);
  position: absolute;
  left: 50%;
  top: -30px;
}
.product-grid .social li a:after {
  content: "";
  height: 15px;
  width: 15px;
  border-radius: 0;
  transform: translateX(-50%) rotate(45deg);
  top: -20px;
  z-index: -1;
}
.product-grid .social li a:hover:after,
.product-grid .social li a:hover:before {
  opacity: 1;
}
.product-grid .product-discount-label,
.product-grid .product-new-label {
  color: #fff;
  background-color: #ef5777;
  font-size: 12px;
  text-transform: uppercase;
  padding: 2px 7px;
  display: block;
  position: absolute;
  top: 10px;
  left: 0;
}
.product-grid .product-discount-label {
  background-color: #333;
  left: auto;
  right: 0;
}
.product-grid .rating {
  color: #ffd200;
  font-size: 12px;
  padding: 12px 0 0;
  margin: 0;
  list-style: none;
  position: relative;
  z-index: -1;
}
.product-grid .rating li.disable {
  color: rgba(0, 0, 0, 0.2);
}
.product-grid .product-content {
  background-color: #fff;
  text-align: center;
  padding: 12px 0;
  margin: 0 auto;
  position: absolute;
  left: 0;
  right: 0;
  bottom: -27px;
  z-index: 1;
  transition: all 0.3s;
}
.product-grid:hover .product-content {
  bottom: 0;
}
.product-grid .title {
  font-size: 13px;
  font-weight: 400;
  letter-spacing: 0.5px;
  text-transform: capitalize;
  margin: 0 0 10px;
  transition: all 0.3s ease 0s;
}
.product-grid .title a {
  color: #828282;
}
.product-grid .title a:hover,
.product-grid:hover .title a {
  color: #ef5777;
}
.product-grid .price {
  color: #333;
  font-size: 17px;
  font-family: Montserrat, sans-serif;
  font-weight: 700;
  letter-spacing: 0.6px;
  margin-bottom: 8px;
  text-align: center;
  transition: all 0.3s;
}
.product-grid .price span {
  color: #999;
  font-size: 13px;
  font-weight: 400;
  text-decoration: line-through;
  margin-left: 3px;
  display: inline-block;
}
.product-grid .add-to-cart {
  color: #000;
  font-size: 13px;
  font-weight: 600;
}
@media only screen and (max-width: 990px) {
  .product-grid {
    margin-bottom: 30px;
  }
}
</style>
