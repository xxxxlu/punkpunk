<template>
  <div class="product-detail" v-if="product">
    <div class="container">
      <div class="breadcrumb">
        <router-link to="/">Home</router-link> /
        <router-link :to="`/?category=${product.category}`">{{ product.category }}</router-link> /
        <span>{{ product.name }}</span>
      </div>

      <div class="product-container">
        <div class="product-image-container">
          <img :src="product.image" :alt="product.name" class="product-image">
        </div>

        <div class="product-info">
          <h1 class="product-name">{{ product.name }}</h1>
          <p class="product-price">Rs. {{ formatPrice(product.price) }}</p>

          <div class="product-status" :class="{ 'out-of-stock': !product.inStock }">
            <i :class="product.inStock ? 'fas fa-check-circle' : 'fas fa-times-circle'"></i>
            {{ product.inStock ? 'In Stock' : 'Out of Stock' }}
          </div>

          <div class="product-description">
            <h3>Product Description</h3>
            <p>{{ product.description }}</p>
          </div>

          <div class="product-actions" v-if="product.inStock">
            <div class="quantity-selector">
              <button @click="decrementQuantity" :disabled="quantity <= 1">
                <i class="fas fa-minus"></i>
              </button>
              <input type="number" v-model.number="quantity" min="1" max="10">
              <button @click="incrementQuantity" :disabled="quantity >= 10">
                <i class="fas fa-plus"></i>
              </button>
            </div>

            <button class="btn btn-primary add-to-cart" @click="addToCart">
              <i class="fas fa-shopping-cart"></i> Add to Cart
            </button>
          </div>

          <div class="out-of-stock-message" v-else>
            <p><i class="fas fa-exclamation-circle"></i> This product is currently out of stock.</p>
          </div>

          <div class="shipping-info">
            <h3>Shipping Information</h3>
            <ul>
              <li><i class="fas fa-truck"></i> Free shipping on orders over Rs. 3,000</li>
              <li><i class="fas fa-clock"></i> Delivery within 3-5 business days</li>
              <li><i class="fas fa-shield-alt"></i> Secure payment</li>
            </ul>
          </div>
        </div>
      </div>

      <!-- Related Products -->
      <div class="related-products" v-if="relatedProducts.length > 0">
        <h2 class="section-title">Related Products</h2>
        <div class="products-grid">
          <ProductCard
            v-for="relatedProduct in relatedProducts"
            :key="relatedProduct.id"
            :product="relatedProduct"
          />
        </div>
      </div>
    </div>
  </div>
  <div class="product-not-found" v-else>
    <div class="container">
      <i class="fas fa-search"></i>
      <h2>Product Not Found</h2>
      <p>The product you are looking for doesn't exist or has been removed.</p>
      <router-link to="/" class="btn btn-primary">Return to Shop</router-link>
    </div>
  </div>
</template>

<script>
import ProductCard from '@/components/ProductCard.vue';
import { mapGetters } from 'vuex';

export default {
  name: 'ProductDetail',
  components: {
    ProductCard
  },
  data() {
    return {
      quantity: 1
    };
  },
  computed: {
    ...mapGetters(['productById']),

    productId() {
      return parseInt(this.$route.params.id);
    },

    product() {
      return this.productById(this.productId);
    },

    relatedProducts() {
      if (!this.product) return [];

      // Get products in the same category
      return this.$store.state.products
        .filter(p => p.category === this.product.category && p.id !== this.product.id)
        .slice(0, 4); // Limit to 4 related products
    }
  },
  methods: {
    formatPrice(price) {
      return price.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    },
    incrementQuantity() {
      if (this.quantity < 10) this.quantity++;
    },
    decrementQuantity() {
      if (this.quantity > 1) this.quantity--;
    },
    addToCart() {
      if (this.product && this.product.inStock) {
        this.$store.dispatch('addToCart', {
          product: this.product,
          quantity: this.quantity
        });
      }
    }
  }
};
</script>

<style scoped>
:root {
  --primary-color: #ff8a8a;
  --primary-hover: #ff6b6b;
  --primary-light: rgba(255, 138, 138, 0.12);
  --primary-lighter: rgba(255, 138, 138, 0.05);
  --success-color: #67c23a;
  --text-dark: #333;
  --text-light: #777;
  --border-light: #eeeeee;
  --shadow-light: 0 4px 12px rgba(0, 0, 0, 0.05);
  --shadow-medium: 0 6px 16px rgba(0, 0, 0, 0.1);
  --transition-normal: all 0.3s ease;
  --gradient-primary: linear-gradient(135deg, #ff8a8a 0%, #ff6b6b 100%);
  --gradient-secondary: linear-gradient(135deg, #ff8a8a 0%, #ff5151 100%);
  --border-radius-full: 30px;
  --shadow-primary: 0 5px 15px rgba(255, 107, 107, 0.3);
}

.product-detail {
  animation: fadeIn 0.8s ease-out;
}

/* 面包屑导航 */
.breadcrumb {
  margin: 24px 0;
  font-size: 0.95rem;
  background-color: rgba(255,255,255,0.8);
  display: inline-flex;
  align-items: center;
  padding: 8px 16px;
  border-radius: 8px;
  box-shadow: var(--shadow-light);
  backdrop-filter: blur(5px);
  animation: slideRight 0.6s ease-out forwards;
  transform: translateX(-10px);
  opacity: 0;
  animation-delay: 0.1s;
}

.breadcrumb a {
  color: var(--text-light);
  text-decoration: none;
  position: relative;
  padding: 0 6px;
  transition: var(--transition-normal);
}

.breadcrumb a::after {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 6px;
  width: calc(100% - 12px);
  height: 2px;
  background-image: var(--gradient-primary);
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.3s ease;
  border-radius: 1px;
}

.breadcrumb a:hover {
  color: var(--primary-color);
}

.breadcrumb a:hover::after {
  transform: scaleX(1);
}

.breadcrumb span {
  color: var(--text-dark);
  font-weight: 600;
  padding: 0 6px;
}

/* 产品容器 */
.product-container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 50px;
  margin-bottom: 80px;
  animation: fadeIn 0.8s ease-out;
}

/* 产品图片 */
.product-image-container {
  background-color: white;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: var(--shadow-medium);
  position: relative;
  transform: translateY(20px);
  opacity: 0;
  animation: slideUp 0.6s ease-out forwards;
  animation-delay: 0.2s;
  transition: transform 0.5s cubic-bezier(0.165, 0.84, 0.44, 1), box-shadow 0.3s ease;
}

.product-image-container::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: radial-gradient(circle at 90% 10%, rgba(255, 107, 107, 0.1) 0%, transparent 60%);
  z-index: 1;
  opacity: 0;
  transition: opacity 0.5s ease;
}

.product-image-container:hover {
  transform: translateY(-5px) scale(1.01);
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.1);
}

.product-image-container:hover::before {
  opacity: 1;
}

.product-image {
  width: 100%;
  height: auto;
  display: block;
  transition: transform 0.8s cubic-bezier(0.165, 0.84, 0.44, 1);
}

.product-image-container:hover .product-image {
  transform: scale(1.05);
}

/* 产品信息 */
.product-info {
  transform: translateY(20px);
  opacity: 0;
  animation: slideUp 0.6s ease-out forwards;
  animation-delay: 0.4s;
}

.product-name {
  font-size: 2.2rem;
  margin-bottom: 15px;
  color: var(--text-dark);
  line-height: 1.3;
  position: relative;
  display: inline-block;
  font-weight: 700;
}

.product-name::after {
  content: '';
  position: absolute;
  bottom: -5px;
  left: 0;
  width: 40px;
  height: 3px;
  background-image: var(--gradient-primary);
  transition: width 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  border-radius: 2px;
}

.product-info:hover .product-name::after {
  width: 100%;
}

.product-price {
  font-size: 1.8rem;
  font-weight: 700;
  background-image: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  color: transparent;
  margin-bottom: 20px;
  display: inline-block;
  padding: 5px 0;
  transform: translateX(-5px);
  opacity: 0;
  animation: slideRight 0.5s ease-out forwards;
  animation-delay: 0.5s;
}

/* 产品状态 */
.product-status {
  display: inline-flex;
  align-items: center;
  background: linear-gradient(to right, rgba(46, 125, 50, 0.1), rgba(46, 125, 50, 0.05));
  color: #2e7d32;
  padding: 8px 16px;
  border-radius: 30px;
  margin-bottom: 25px;
  font-weight: 500;
  box-shadow: 0 3px 8px rgba(46, 125, 50, 0.2);
  transform: scale(0.9);
  animation: pulseScale 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
  animation-delay: 0.6s;
}

.product-status i {
  margin-right: 10px;
  font-size: 1.1rem;
}

.product-status.out-of-stock {
  background: linear-gradient(to right, rgba(198, 40, 40, 0.1), rgba(198, 40, 40, 0.05));
  color: #c62828;
  box-shadow: 0 3px 8px rgba(198, 40, 40, 0.2);
}

/* 产品描述 */
.product-description {
  margin-bottom: 30px;
  transform: translateY(10px);
  opacity: 0;
  animation: slideUp 0.5s ease-out forwards;
  animation-delay: 0.7s;
}

.product-description h3 {
  font-size: 1.2rem;
  margin-bottom: 12px;
  color: var(--text-dark);
  position: relative;
  display: inline-block;
  padding-bottom: 6px;
}

.product-description h3::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 30px;
  height: 2px;
  background-color: var(--primary-color);
}

.product-description p {
  line-height: 1.8;
  color: var(--text-light);
  font-size: 1.05rem;
}

/* 产品操作 */
.product-actions {
  display: flex;
  gap: 15px;
  margin-bottom: 30px;
  align-items: center;
  transform: translateY(10px);
  opacity: 0;
  animation: slideUp 0.5s ease-out forwards;
  animation-delay: 0.8s;
}

/* 数量选择器 */
.quantity-selector {
  display: flex;
  border: 1px solid var(--border-light);
  border-radius: 8px;
  overflow: hidden;
  box-shadow: var(--shadow-light);
}

.quantity-selector button {
  width: 40px;
  height: 45px;
  background: linear-gradient(to bottom, #ffffff, #f7f7f7);
  border: none;
  cursor: pointer;
  transition: var(--transition-normal);
  color: var(--text-dark);
}

.quantity-selector button:hover {
  background: linear-gradient(to bottom, #f5f5f5, #e8e8e8);
  color: var(--primary-color);
}

.quantity-selector button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.quantity-selector input {
  width: 60px;
  text-align: center;
  border: none;
  border-left: 1px solid var(--border-light);
  border-right: 1px solid var(--border-light);
  font-size: 1.1rem;
  color: var(--text-dark);
  font-weight: 600;
}

/* 添加到购物车按钮 */
.add-to-cart {
  padding: 0 28px;
  height: 45px;
  font-weight: 600;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  border-radius: var(--border-radius-full);
  background-image: var(--gradient-primary);
  color: white;
  border: none;
  cursor: pointer;
  box-shadow: var(--shadow-primary);
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.add-to-cart i {
  font-size: 0.9rem;
  transition: transform 0.3s ease;
}

.add-to-cart:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 20px rgba(255, 107, 107, 0.4);
}

.add-to-cart:hover i {
  transform: translateX(-3px);
}

/* 缺货信息 */
.out-of-stock-message {
  background: linear-gradient(to right, rgba(198, 40, 40, 0.1), rgba(198, 40, 40, 0.02));
  padding: 20px;
  border-radius: 12px;
  margin-bottom: 30px;
  border-left: 3px solid #c62828;
  animation: pulse 2s infinite;
  transform: translateY(10px);
  opacity: 0;
  animation-name: slideUp;
  animation-duration: 0.5s;
  animation-timing-function: ease-out;
  animation-fill-mode: forwards;
  animation-delay: 0.8s;
}

.out-of-stock-message p {
  display: flex;
  align-items: center;
  color: #c62828;
  font-weight: 500;
  font-size: 1.05rem;
}

.out-of-stock-message i {
  margin-right: 12px;
  font-size: 1.2rem;
}

/* 配送信息 */
.shipping-info {
  background-color: white;
  padding: 25px;
  border-radius: 12px;
  box-shadow: var(--shadow-light);
  transform: translateY(10px);
  opacity: 0;
  animation: slideUp 0.5s ease-out forwards;
  animation-delay: 0.9s;
  position: relative;
  overflow: hidden;
}

.shipping-info::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 4px;
  background-image: var(--gradient-primary);
}

.shipping-info h3 {
  font-size: 1.2rem;
  margin-bottom: 18px;
  color: var(--text-dark);
  position: relative;
  display: inline-block;
}

.shipping-info ul {
  list-style: none;
  padding: 0;
}

.shipping-info li {
  margin-bottom: 16px;
  display: flex;
  align-items: center;
  color: var(--text-light);
  transition: transform 0.3s ease;
}

.shipping-info li:hover {
  transform: translateX(5px);
  color: var(--text-dark);
}

.shipping-info i {
  background-image: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  color: transparent;
  margin-right: 12px;
  font-size: 1.1rem;
}

/* 相关产品 */
.related-products {
  margin-bottom: 80px;
  transform: translateY(20px);
  opacity: 0;
  animation: slideUp 0.6s ease-out forwards;
  animation-delay: 1s;
}

.related-products .section-title {
  font-size: 1.8rem;
  margin-bottom: 2rem;
  color: var(--text-dark);
  position: relative;
  padding-bottom: 10px;
}

.related-products .section-title::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 60px;
  height: 3px;
  background-color: var(--primary-color);
  border-radius: 3px;
}

.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 30px;
}

/* 产品未找到 */
.product-not-found {
  text-align: center;
  padding: 100px 0;
  animation: fadeIn 0.8s ease-out;
  background: linear-gradient(145deg, white, #f8f9fa);
  border-radius: 20px;
  box-shadow: var(--shadow-light);
  margin: 40px 0;
}

.product-not-found i {
  font-size: 5rem;
  color: #e0e0e0;
  margin-bottom: 1.5rem;
  animation: bounce 2s infinite;
}

.product-not-found h2 {
  font-size: 2rem;
  margin-bottom: 15px;
  color: var(--text-dark);
}

.product-not-found p {
  color: var(--text-light);
  margin-bottom: 25px;
  max-width: 500px;
  margin-left: auto;
  margin-right: auto;
  font-size: 1.1rem;
}

.product-not-found .btn {
  padding: 12px 30px;
  border-radius: var(--border-radius-full);
  background-image: var(--gradient-primary);
  color: white;
  font-weight: 600;
  text-decoration: none;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  box-shadow: var(--shadow-primary);
  transition: all 0.3s ease;
}

.product-not-found .btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 20px rgba(255, 107, 107, 0.4);
}

/* 动画 */
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideUp {
  from { 
    opacity: 0; 
    transform: translateY(20px);
  }
  to { 
    opacity: 1; 
    transform: translateY(0);
  }
}

@keyframes slideRight {
  from { 
    opacity: 0; 
    transform: translateX(-10px);
  }
  to { 
    opacity: 1; 
    transform: translateX(0);
  }
}

@keyframes pulseScale {
  from { 
    opacity: 0; 
    transform: scale(0.9);
  }
  to { 
    opacity: 1; 
    transform: scale(1);
  }
}

@keyframes bounce {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-15px);
  }
}

@keyframes pulse {
  0% {
    box-shadow: 0 0 0 0 rgba(198, 40, 40, 0.4);
  }
  70% {
    box-shadow: 0 0 0 10px rgba(198, 40, 40, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(198, 40, 40, 0);
  }
}

/* 响应式布局 */
@media (max-width: 992px) {
  .product-container {
    grid-template-columns: 1fr;
  }
  
  .product-name {
    font-size: 1.8rem;
  }
  
  .product-price {
    font-size: 1.5rem;
  }
}

@media (max-width: 768px) {
  .breadcrumb {
    font-size: 0.9rem;
    margin: 16px 0;
  }
  
  .product-image-container {
    margin-bottom: 20px;
  }
  
  .product-description p {
    font-size: 1rem;
  }
  
  .product-actions {
    flex-direction: column;
    align-items: flex-start;
  }
  
  .quantity-selector {
    width: 100%;
    margin-bottom: 15px;
  }
  
  .add-to-cart {
    width: 100%;
  }
  
  .shipping-info li {
    font-size: 0.95rem;
  }
}
</style>
