<template>
  <div class="checkout-page">
    <div class="container">
      <h1 class="section-title">Checkout</h1>

      <div class="checkout-empty" v-if="cart.length === 0">
        <i class="fas fa-shopping-bag"></i>
        <h2>Your cart is empty</h2>
        <p>You need to add products to your cart before proceeding to checkout.</p>
        <router-link to="/" class="btn btn-primary">Shop Now</router-link>
      </div>

      <div class="checkout-container" v-else>
        <!-- Checkout Steps Progress -->
        <div class="checkout-progress">
          <div class="progress-step" :class="{ 'active': checkout.step >= 1, 'completed': checkout.step > 1 }">
            <div class="step-number">1</div>
            <div class="step-label">Products</div>
          </div>
          <div class="progress-bar" :class="{ 'active': checkout.step > 1 }"></div>
          <div class="progress-step" :class="{ 'active': checkout.step >= 2, 'completed': checkout.step > 2 }">
            <div class="step-number">2</div>
            <div class="step-label">Information</div>
          </div>
          <div class="progress-bar" :class="{ 'active': checkout.step > 2 }"></div>
          <div class="progress-step" :class="{ 'active': checkout.step >= 3 }">
            <div class="step-number">3</div>
            <div class="step-label">Confirmation</div>
          </div>
        </div>

        <!-- Products Review (Step 1) -->
        <div class="checkout-content">
          <div class="products-summary">
            <h2>Order Summary</h2>

            <div class="checkout-items">
              <div class="checkout-item" v-for="item in cart" :key="item.id">
                <div class="item-image">
                  <img :src="item.image" :alt="item.name">
                  <span class="item-quantity">{{ item.quantity }}</span>
                </div>

                <div class="item-details">
                  <div class="item-name">{{ item.name }}</div>
                  <div class="item-category">{{ item.category }}</div>
                </div>

                <div class="item-price">
                  Rs. {{ formatPrice(item.price * item.quantity) }}
                </div>
              </div>
            </div>

            <div class="checkout-summary">
              <div class="summary-row">
                <div class="summary-label">Subtotal</div>
                <div class="summary-value">Rs. {{ formatPrice(cartTotal) }}</div>
              </div>

              <div class="summary-row">
                <div class="summary-label">Shipping</div>
                <div class="summary-value">{{ shippingCost === 0 ? 'Free' : `Rs. ${formatPrice(shippingCost)}` }}</div>
              </div>

              <div class="summary-row total">
                <div class="summary-label">Total</div>
                <div class="summary-value">Rs. {{ formatPrice(cartTotal + shippingCost) }}</div>
              </div>
            </div>

            <div class="checkout-actions">
              <router-link to="/cart" class="btn btn-secondary">
                <i class="fas fa-arrow-left"></i> Back to Cart
              </router-link>

              <router-link to="/checkout/information" class="btn btn-primary" @click.native="setCheckoutStep(2)">
                Continue to Information <i class="fas fa-arrow-right"></i>
              </router-link>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState, mapGetters, mapActions } from 'vuex';

export default {
  name: 'Checkout',
  computed: {
    ...mapState(['cart', 'checkout']),
    ...mapGetters(['cartTotal']),

    shippingCost() {
      return this.cartTotal >= 3000 ? 0 : 300;
    }
  },
  methods: {
    ...mapActions(['setCheckoutStep']),

    formatPrice(price) {
      return price.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    }
  },
  created() {
    // Load cart from localStorage if available
    this.$store.dispatch('loadCart');

    // Set checkout step to 1
    this.setCheckoutStep(1);
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
  --success-gradient: linear-gradient(135deg, #67c23a 0%, #52b625 100%);
  --success-shadow: 0 5px 15px rgba(103, 194, 58, 0.3);
}

.checkout-page {
  padding: 40px 0 80px;
  animation: fadeIn 0.8s ease-out;
}

.section-title {
  font-size: 2.2rem;
  margin-bottom: 2rem;
  color: var(--text-dark);
  position: relative;
  padding-bottom: 12px;
  display: inline-block;
  transform: translateY(10px);
  opacity: 0;
  animation: slideDown 0.5s ease-out forwards;
}

.section-title::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 60px;
  height: 3px;
  background-image: var(--gradient-primary);
  border-radius: 2px;
  transition: width 0.4s cubic-bezier(0.165, 0.84, 0.44, 1);
}

.section-title:hover::after {
  width: 100%;
}

/* 空购物车样式 */
.checkout-empty {
  text-align: center;
  padding: 70px 0;
  background: linear-gradient(145deg, white, #f8f9fa);
  border-radius: 16px;
  box-shadow: var(--shadow-light);
  margin: 20px 0;
  transform: translateY(20px);
  opacity: 0;
  animation: slideUp 0.6s ease-out forwards;
  animation-delay: 0.2s;
}

.checkout-empty i {
  font-size: 5rem;
  color: #e0e0e0;
  margin-bottom: 1.5rem;
  animation: float 3s ease-in-out infinite;
}

.checkout-empty h2 {
  font-size: 2rem;
  margin-bottom: 15px;
  color: var(--text-dark);
  font-weight: 600;
}

.checkout-empty p {
  color: var(--text-light);
  margin-bottom: 25px;
  font-size: 1.1rem;
  max-width: 500px;
  margin-left: auto;
  margin-right: auto;
}

.checkout-empty .btn {
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
  border: none;
}

.checkout-empty .btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 20px rgba(255, 107, 107, 0.4);
}

.checkout-empty .btn i {
  font-size: 1rem;
  margin-right: 5px;
  animation: none;
  color: white;
}

.checkout-container {
  max-width: 800px;
  margin: 0 auto;
  opacity: 0;
  transform: translateY(20px);
  animation: slideUp 0.6s ease-out forwards;
  animation-delay: 0.3s;
}

/* 进度条 */
.checkout-progress {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 40px;
  padding: 20px;
  background-color: white;
  border-radius: 16px;
  box-shadow: var(--shadow-light);
}

.progress-step {
  display: flex;
  flex-direction: column;
  align-items: center;
  flex: 0 0 auto;
  position: relative;
  z-index: 2;
  transition: transform 0.3s ease;
}

.progress-step:hover {
  transform: translateY(-5px);
}

.step-number {
  width: 45px;
  height: 45px;
  border-radius: 50%;
  background-color: #f5f5f5;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  margin-bottom: 10px;
  color: var(--text-light);
  border: 2px solid var(--border-light);
  transition: all 0.4s cubic-bezier(0.165, 0.84, 0.44, 1);
  box-shadow: var(--shadow-light);
}

.step-label {
  font-size: 0.95rem;
  color: var(--text-light);
  font-weight: 500;
  transition: var(--transition-normal);
}

.progress-bar {
  flex-grow: 1;
  height: 4px;
  background-color: #f0f0f0;
  margin: 0 15px;
  margin-bottom: 28px;
  position: relative;
  z-index: 1;
  border-radius: 2px;
  overflow: hidden;
  transition: background-color 0.5s ease;
}

.progress-bar::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 0;
  background-image: var(--gradient-primary);
  transition: width 0.6s cubic-bezier(0.165, 0.84, 0.44, 1);
  border-radius: 2px;
}

.progress-step.active .step-number {
  background-image: var(--gradient-primary);
  color: white;
  border-color: var(--primary-hover);
  box-shadow: var(--shadow-primary);
  animation: pulse 2s infinite;
}

.progress-step.active .step-label {
  color: var(--text-dark);
  font-weight: 600;
}

.progress-step.completed .step-number {
  background-image: var(--success-gradient);
  color: white;
  border-color: var(--success-color);
  box-shadow: var(--success-shadow);
}

.progress-step.completed .step-label {
  color: var(--success-color);
}

.progress-bar.active::before {
  width: 100%;
}

/* 结账内容 */
.checkout-content {
  background-color: white;
  border-radius: 16px;
  box-shadow: var(--shadow-medium);
  padding: 35px;
  transform: translateY(20px);
  opacity: 0;
  animation: slideUp 0.6s ease-out forwards;
  animation-delay: 0.5s;
  position: relative;
  overflow: hidden;
}

.checkout-content::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 4px;
  background-image: var(--gradient-primary);
}

.products-summary h2 {
  font-size: 1.5rem;
  margin-bottom: 25px;
  padding-bottom: 12px;
  border-bottom: 1px solid var(--border-light);
  color: var(--text-dark);
  position: relative;
  display: inline-block;
}

.products-summary h2::after {
  content: '';
  position: absolute;
  bottom: -1px;
  left: 0;
  width: 50px;
  height: 3px;
  background-image: var(--gradient-primary);
  border-radius: 2px;
  transition: width 0.4s cubic-bezier(0.165, 0.84, 0.44, 1);
}

.products-summary h2:hover::after {
  width: 100%;
}

.checkout-items {
  margin-bottom: 35px;
}

.checkout-item {
  display: grid;
  grid-template-columns: auto 1fr auto;
  gap: 20px;
  padding: 20px 0;
  border-bottom: 1px solid var(--border-light);
  align-items: center;
  transition: var(--transition-normal);
  position: relative;
  overflow: hidden;
}

.checkout-item::before {
  content: '';
  position: absolute;
  left: -5px;
  top: 0;
  height: 100%;
  width: 3px;
  background-image: var(--gradient-primary);
  transform: scaleY(0);
  transition: transform 0.3s ease, left 0.3s ease;
}

.checkout-item:hover {
  background-color: var(--primary-lighter);
  transform: translateX(5px);
}

.checkout-item:hover::before {
  transform: scaleY(1);
  left: 0;
}

.checkout-item:last-child {
  border-bottom: none;
}

.item-image {
  width: 70px;
  height: 70px;
  border-radius: 10px;
  overflow: hidden;
  position: relative;
  box-shadow: var(--shadow-light);
  transition: transform 0.3s cubic-bezier(0.165, 0.84, 0.44, 1), box-shadow 0.3s ease;
}

.checkout-item:hover .item-image {
  transform: translateY(-5px) scale(1.05);
  box-shadow: var(--shadow-medium);
}

.item-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.5s cubic-bezier(0.165, 0.84, 0.44, 1);
}

.checkout-item:hover .item-image img {
  transform: scale(1.1);
}

.item-quantity {
  position: absolute;
  top: -5px;
  right: -5px;
  background-image: var(--gradient-primary);
  color: white;
  width: 24px;
  height: 24px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.85rem;
  font-weight: 600;
  box-shadow: 0 3px 8px rgba(255, 107, 107, 0.4);
  transition: transform 0.3s ease;
}

.checkout-item:hover .item-quantity {
  transform: scale(1.1) translateY(-2px);
}

.item-name {
  font-weight: 600;
  margin-bottom: 8px;
  color: var(--text-dark);
  transition: color 0.3s ease;
  position: relative;
  display: inline-block;
}

.item-name::after {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 0;
  width: 0;
  height: 2px;
  background-image: var(--gradient-primary);
  transition: width 0.3s ease;
  border-radius: 1px;
}

.checkout-item:hover .item-name {
  color: var(--primary-color);
}

.checkout-item:hover .item-name::after {
  width: 100%;
}

.item-category {
  font-size: 0.95rem;
  color: var(--text-light);
  transition: var(--transition-normal);
}

.checkout-item:hover .item-category {
  color: var(--text-dark);
}

.item-price {
  font-weight: 700;
  background-image: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  color: transparent;
  font-size: 1.1rem;
  transition: transform 0.3s ease;
}

.checkout-item:hover .item-price {
  transform: scale(1.05);
}

.checkout-summary {
  background: linear-gradient(145deg, #ffffff, #f8f9fa);
  border-radius: 14px;
  padding: 25px;
  margin-bottom: 35px;
  box-shadow: var(--shadow-light);
  position: relative;
  overflow: hidden;
  border-left: 3px solid var(--primary-color);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.checkout-summary:hover {
  transform: translateY(-5px);
  box-shadow: var(--shadow-medium);
}

.summary-row {
  display: flex;
  justify-content: space-between;
  margin-bottom: 18px;
  padding-bottom: 18px;
  border-bottom: 1px solid var(--border-light);
  transition: transform 0.3s ease;
}

.summary-row:hover {
  transform: translateX(5px);
}

.summary-row .summary-label {
  color: var(--text-light);
  font-weight: 500;
}

.summary-row .summary-value {
  font-weight: 600;
  color: var(--text-dark);
}

.summary-row:last-of-type {
  border-bottom: none;
  margin-bottom: 0;
  padding-bottom: 0;
}

.summary-row.total {
  font-weight: 700;
  font-size: 1.2rem;
  color: var(--text-dark);
  margin-top: 15px;
  border-top: 2px solid var(--border-light);
  padding-top: 15px;
}

.summary-row.total .summary-value {
  background-image: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  color: transparent;
}

.checkout-actions {
  display: flex;
  justify-content: space-between;
  gap: 20px;
}

.checkout-actions .btn {
  padding: 12px 25px;
  border-radius: var(--border-radius-full);
  font-weight: 600;
  text-decoration: none;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  transition: all 0.3s ease;
  border: none;
}

.checkout-actions .btn-primary {
  background-image: var(--gradient-primary);
  color: white;
  box-shadow: var(--shadow-primary);
  flex: 1.5;
}

.checkout-actions .btn-primary:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 20px rgba(255, 107, 107, 0.4);
}

.checkout-actions .btn-primary i {
  transition: transform 0.3s ease;
}

.checkout-actions .btn-primary:hover i {
  transform: translateX(3px);
}

.checkout-actions .btn-secondary {
  background-color: white;
  color: var(--text-dark);
  border: 1px solid var(--border-light);
  flex: 1;
}

.checkout-actions .btn-secondary:hover {
  background-color: #f8f9fa;
  color: var(--primary-color);
  transform: translateY(-3px);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
}

.checkout-actions .btn-secondary i {
  transition: transform 0.3s ease;
}

.checkout-actions .btn-secondary:hover i {
  transform: translateX(-3px);
}

/* 动画效果 */
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

@keyframes slideDown {
  from { 
    opacity: 0; 
    transform: translateY(-20px);
  }
  to { 
    opacity: 1; 
    transform: translateY(0);
  }
}

@keyframes float {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}

@keyframes pulse {
  0% {
    box-shadow: 0 0 0 0 rgba(255, 107, 107, 0.4);
  }
  70% {
    box-shadow: 0 0 0 10px rgba(255, 107, 107, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(255, 107, 107, 0);
  }
}

/* 响应式布局 */
@media (max-width: 768px) {
  .step-label {
    font-size: 0.85rem;
  }

  .progress-bar {
    margin: 0 10px;
    margin-bottom: 28px;
  }

  .checkout-item {
    grid-template-columns: 60px 1fr;
    padding: 15px 0;
    gap: 15px;
  }

  .item-image {
    width: 60px;
    height: 60px;
  }

  .item-price {
    grid-column: 2;
    text-align: right;
    margin-top: 8px;
  }

  .checkout-actions {
    flex-direction: column;
    gap: 15px;
  }

  .checkout-actions .btn {
    width: 100%;
    text-align: center;
  }
  
  .checkout-content {
    padding: 25px 20px;
  }
  
  .section-title {
    font-size: 1.8rem;
  }
}

@media (max-width: 576px) {
  .checkout-progress {
    padding: 15px 10px;
  }
  
  .step-number {
    width: 35px;
    height: 35px;
    font-size: 0.9rem;
  }
  
  .step-label {
    font-size: 0.8rem;
  }
  
  .progress-bar {
    margin: 0 5px;
    margin-bottom: 28px;
  }
  
  .checkout-page {
    padding: 20px 0 50px;
  }
  
  .checkout-empty {
    padding: 50px 20px;
  }
  
  .checkout-empty h2 {
    font-size: 1.5rem;
  }
  
  .checkout-empty p {
    font-size: 1rem;
  }
}
</style>
