<template>
  <div class="product-card">
    <router-link :to="`/product/${product.id}`" class="product-image">
      <img :src="product.image" :alt="product.name">
      <span v-if="!product.inStock" class="out-of-stock">Out of Stock</span>
    </router-link>

    <div class="product-info">
      <div class="product-category">{{ product.category }}</div>
      <router-link :to="`/product/${product.id}`" class="product-name">{{ product.name }}</router-link>
      <div class="product-price">Rs. {{ formatPrice(product.price) }}</div>

      <button
        class="btn btn-primary btn-block add-to-cart"
        @click="addToCart"
        :disabled="!product.inStock">
        <i class="fas fa-shopping-cart"></i>
        {{ product.inStock ? 'Add to Cart' : 'Out of Stock' }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProductCard',
  props: {
    product: {
      type: Object,
      required: true
    }
  },
  methods: {
    formatPrice(price) {
      return price.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    },
    addToCart() {
      if (this.product.inStock) {
        this.$store.dispatch('addToCart', {
          product: this.product,
          quantity: 1
        });
      }
    }
  }
};
</script>

<style scoped>
:root {
  --gradient-primary: linear-gradient(135deg, #ff8a8a 0%, #ff6b6b 100%);
  --gradient-secondary: linear-gradient(135deg, #ff8a8a 0%, #ff5151 100%);
  --shadow-sm: 0 4px 6px rgba(0, 0, 0, 0.05), 0 1px 3px rgba(0, 0, 0, 0.1);
  --shadow-md: 0 10px 20px rgba(0, 0, 0, 0.06), 0 6px 6px rgba(0, 0, 0, 0.1);
  --shadow-primary: 0 5px 15px rgba(255, 107, 107, 0.3);
  --border-radius-sm: 5px;
  --border-radius-md: 8px;
  --border-radius-lg: 12px;
  --border-radius-full: 20px;
  --transition-ease: all 0.3s ease;
  --transition-bounce: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.product-card {
  background-color: white;
  border-radius: var(--border-radius-lg);
  box-shadow: var(--shadow-sm);
  overflow: hidden;
  transition: var(--transition-bounce);
  height: 100%;
  display: flex;
  flex-direction: column;
  position: relative;
  border: 1px solid rgba(0, 0, 0, 0.03);
}

.product-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 4px;
  background-image: var(--gradient-primary);
  transform: scaleX(0);
  transform-origin: right;
  transition: transform 0.5s ease-out;
  z-index: 1;
}

.product-card:hover {
  transform: translateY(-10px);
  box-shadow: var(--shadow-md);
  border-color: rgba(255, 107, 107, 0.1);
}

.product-card:hover::before {
  transform: scaleX(1);
  transform-origin: left;
}

.product-image {
  position: relative;
  padding-top: 100%;
  display: block;
  overflow: hidden;
  background-color: #f8f8f8;
}

.product-image::after {
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

.product-card:hover .product-image::after {
  opacity: 1;
}

.product-image img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.7s cubic-bezier(0.165, 0.84, 0.44, 1);
}

.product-card:hover .product-image img {
  transform: scale(1.08);
}

.out-of-stock {
  position: absolute;
  top: 10px;
  right: 10px;
  background-image: var(--gradient-primary);
  color: white;
  padding: 5px 12px;
  border-radius: var(--border-radius-full);
  font-size: 0.75rem;
  font-weight: 600;
  z-index: 2;
  box-shadow: 0 3px 6px rgba(255, 107, 107, 0.3);
  animation: pulseLight 2s infinite;
}

@keyframes pulseLight {
  0% { box-shadow: 0 0 0 0 rgba(255, 107, 107, 0.5); }
  70% { box-shadow: 0 0 0 8px rgba(255, 107, 107, 0); }
  100% { box-shadow: 0 0 0 0 rgba(255, 107, 107, 0); }
}

.product-info {
  padding: 20px;
  display: flex;
  flex-direction: column;
  flex-grow: 1;
  position: relative;
  z-index: 2;
}

.product-category {
  color: rgba(0, 0, 0, 0.5);
  font-size: 0.8rem;
  margin-bottom: 8px;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  font-weight: 500;
  transition: var(--transition-ease);
}

.product-card:hover .product-category {
  color: rgba(255, 107, 107, 0.8);
}

.product-name {
  font-weight: 600;
  font-size: 1.05rem;
  color: #333;
  text-decoration: none;
  margin-bottom: 10px;
  display: block;
  line-height: 1.4;
  height: 2.8em;
  overflow: hidden;
  transition: var(--transition-ease);
  position: relative;
}

.product-name::after {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 0;
  width: 40px;
  height: 2px;
  background-image: var(--gradient-primary);
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.5s ease;
}

.product-card:hover .product-name {
  color: #111;
}

.product-card:hover .product-name::after {
  transform: scaleX(1);
}

.product-price {
  font-weight: 700;
  font-size: 1.15rem;
  background-image: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  color: transparent;
  margin-bottom: 18px;
  display: inline-block;
  padding: 2px 0;
}

.add-to-cart {
  margin-top: auto;
  padding: 12px 22px;
  border-radius: var(--border-radius-full);
  background-image: var(--gradient-primary);
  color: white;
  font-weight: 600;
  border: none;
  cursor: pointer;
  box-shadow: var(--shadow-primary);
  transition: var(--transition-bounce);
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.add-to-cart i {
  font-size: 0.9rem;
  transition: transform 0.3s ease;
}

.add-to-cart:hover {
  box-shadow: 0 8px 15px rgba(255, 107, 107, 0.5);
  transform: translateY(-3px);
}

.add-to-cart:hover i {
  transform: translateX(-3px);
}

.add-to-cart:disabled {
  background: linear-gradient(135deg, #d4d4d4 0%, #a0a0a0 100%);
  cursor: not-allowed;
  box-shadow: none;
}

.add-to-cart:disabled:hover {
  transform: none;
  box-shadow: none;
}

@media (max-width: 768px) {
  .product-info {
    padding: 15px;
  }
  
  .product-name {
    font-size: 1rem;
  }
  
  .add-to-cart {
    padding: 10px 18px;
  }
}
</style>
