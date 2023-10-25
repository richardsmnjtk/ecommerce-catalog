<template>
  <div v-if="isLoading" class="container">
    <div class="product__container">
      <div class="product__image skeleton">image</div>
      <div class="product__content">
        <div>
          <h3 class="product__title skeleton">title</h3>
          <div class="product__rating skeleton">rating</div>
          <hr>
          <p class="product__description skeleton">description</p>
        </div>
        <div>
          <hr>
          <p class="product__price skeleton">price</p>
          <div class="product__action">
            <button class="primary-button skeleton">Buy Now</button>
            <button class="secondary-button skeleton">Next Product</button>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div 
    v-else
    class="container"
    :class="
      !isProductAvailable
      ? 'bg-light-grey'
      : data.product.category === 'men\'s clothing'
      ? 'bg-light-blue font-blue'
      : 'bg-light-magenta font-magenta'
    "
  >
    <div v-if="!isProductAvailable" class="unavailable__container">
      <img src="../assets/image/sad-face.png" class="unavailable__img">
      <div class="unavailable__content">
        <p class="unavailable__text">This product is unavailable to show</p>
        <button class="unavailable-button" @click="getProductById()">Next Product</button>
      </div>
    </div>
    <div v-else class="product__container">
      <img :src="data.product.image" class="product__image" alt="product-image" />
      <div class="product__content">
        <div>
          <h3 class="product__title">{{ data.product.title }}</h3>
          <div style="display: flex; justify-content: space-between; align-items: center; font-size: 18px;">
            <p class="product__category">{{ data.product.category }}</p>
            <div class="product__rating">
              <span>{{ data.product.rating.rate }}</span>
              <div class="dots">
                <span 
                  v-for="dot in 5"
                  :key="dot"
                  :class="
                    data.product.category === 'men\'s clothing'
                    ? ['dot', { 'dot-blue': dot <= Math.round(data.product.rating.rate) }]
                    : ['dot', { 'dot-magenta': dot <= Math.round(data.product.rating.rate) }]
                  "
                >
                </span>
              </div>
            </div>
          </div>
          <hr>
          <p class="product__description">{{ data.product.description }}</p>
        </div>
        <div>
          <hr>
          <p class="product__price">${{ data.product.price }}</p>
          <div class="product__action">
            <button 
              class="primary-button" 
              :class="data.product.category === 'men\'s clothing' ? 'primary-btn-blue' : 'primary-btn-magenta'"
            >
              Buy Now
            </button>
            <button 
              class="secondary-button" 
              :class="data.product.category === 'men\'s clothing' ? 'secondary-btn-blue' : 'secondary-btn-magenta'"
              @click="getProductById()"
            >
              Next Product
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProductDisplay',
  data () {
    return {
      data: {},
      index: 0,
      isLoading: false,
      isProductAvailable: false
    }
  },
  methods: {
    async callAPI() {
      const response = await fetch(`https://fakestoreapi.com/products/${this.index}`);
      const result = await response.json();

      return result;
    },
    async getProductById() {
      this.isLoading = true;
      this.index == 20 ? (this.index = 1) : this.index++;

      let product = await this.callAPI();

      if(product.category === "men's clothing" || product.category === "women's clothing") {
        this.data = { product };
        this.isProductAvailable = true;
      } else {
        this.isProductAvailable = false;
      }
      this.isLoading = false;
    },
  },
  created() {
    this.getProductById();
  }
}
</script>
<style>
@import '../assets/style/page.css';
</style>