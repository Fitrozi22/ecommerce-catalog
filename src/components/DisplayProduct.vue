<script>
import axios from 'axios';

export default {
  name: 'DisplayPage',
  data() {
    return {
      isLoading: false,
      index: 0,
      isProductAvailable: false,
      product: {}
    }
  },
  methods: {
    async getDATA() {
      try {
        const response = await axios.get(`https://fakestoreapi.com/products/${this.index}`);
        return response.data;
      } catch (error) {
        console.error('Error fetching data:', error);
        throw error;
      }
    },
    async getSingleProduct() {
      this.isLoading = true;

      if (this.index !== 20) {
        this.index++
      } else {
        this.index = 1;
      }

      try {
        let data = await this.getDATA();

        if (data.category === "men's clothing" || data.category === "women's clothing" ) {
          this.product = { data }
          this.isProductAvailable = true;
        } else {
          this.isProductAvailable = false;
        }
      } catch (error) {
        // Handle errors here
      }

      this.isLoading = false;
    }
  },
  mounted() {
    this.getSingleProduct();
  },
}
</script>

<template>
    <div class="container">
        <!--Loading-->
        <div v-if="isLoading" class="card">
            <div class="container-product">
                <div class="loading-thumbnail"></div>
                <div class="loading-details">
                    <div class="loading-details-top"></div>
                    <div class="loading-details-bottom"></div>
                </div>
            </div>
        </div>
        <!--Product Unavailable-->
        <div v-else class="container" :class="!isProductAvailable ? 'bg-gray' : product.data.category === 'men\'s clothing' ? 'bg-blue' : 'bg-pink'">
            <div class="expanse">
                <img src="" alt="background expanse">
            </div>
            <div class="card">
                <div v-if="!isProductAvailable" class="product-unavailable-container">
                    <div class="expanse">
                        <img src="" alt="" srcset="">
                    </div>
                    <div class="product-details">
                        <p>This product is unavailable to show</p>
                        <div class="cta">
                            <button type="button" @click="getSingleProduct()" class="cta-next">Next Product</button>
                        </div>
                    </div>
                </div>
                <!--Product Available-->
                <div v-else class="container-product">
                    <div class="product-thumbnail">
                        <img :src="product.data.image" alt="">
                    </div>
                    <div class="product-details">
                        <div class="top">
                            <h3 :class="product.data.category === 'men\'s clothing' ? 'font-navy' : 'font-magenta'" class="title">{{ product.data.title }}</h3>
                            <div class="sub-title">
                                <span>{{ product.data.category }}</span>
                                <div class="rating">
                                    <span>{{ product.data.rating.rate }}/5</span>
                                    <div class="rating">
                                        <span :class="product.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-magenta'" class="circle"></span>
                                        <span :class="product.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-magenta'" class="circle"></span>
                                        <span :class="product.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-magenta'" class="circle"></span>
                                        <span :class="product.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-magenta'" class="circle"></span>
                                        <span :class="product.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-magenta'" class="circle"></span>
                                    </div>
                                </div>
                            </div>
                            <div class="description">
                                <p>{{ product.data.description }}</p>
                            </div>
                        </div>
                        <div class="bottom">
                            <span :class="product.data.category === 'men\'s clothing' ? 'font-navy' : 'font-magenta'" class="price">${{ product.data.price }}</span>
                            <div class="cta">
                                <button type="button" :class="product.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-magenta'" class="cta-buy">Buy Now</button>
                                <button type="button" @click="getSingleProduct()" :class="product.data.category === 'men\'s clothing'  ? 'border-navy font-navy' : 'border-magenta font-magenta'" class="cta-next">Next Product</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>



<style scoped>
    @import '../assets/main.css';
</style>
