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
          <div class="progress-step completed">
            <div class="step-number">1</div>
            <div class="step-label">Products</div>
          </div>
          <div class="progress-bar active"></div>
          <div class="progress-step active">
            <div class="step-number">2</div>
            <div class="step-label">Information</div>
          </div>
          <div class="progress-bar"></div>
          <div class="progress-step">
            <div class="step-number">3</div>
            <div class="step-label">Confirmation</div>
          </div>
        </div>

        <!-- Customer Information (Step 2) -->
        <div class="checkout-content">
          <div class="information-form">
            <h2>Contact Information</h2>

            <form @submit.prevent="submitForm">
              <div class="form-grid">
                <div class="form-group">
                  <label for="name">Full Name <span class="required">*</span></label>
                  <input
                    type="text"
                    id="name"
                    v-model="customerInfo.name"
                    required
                    placeholder="Enter your full name"
                  >
                </div>

                <div class="form-group">
                  <label for="email">Email Address <span class="required">*</span></label>
                  <input
                    type="email"
                    id="email"
                    v-model="customerInfo.email"
                    required
                    placeholder="Enter your email"
                  >
                </div>

                <div class="form-group">
                  <label for="phone">Phone Number <span class="required">*</span></label>
                  <input
                    type="tel"
                    id="phone"
                    v-model="customerInfo.phone"
                    required
                    placeholder="Enter your phone number"
                  >
                </div>
              </div>

              <h2>Shipping Information</h2>

              <div class="form-grid">
                <div class="form-group full-width">
                  <label for="address">Address <span class="required">*</span></label>
                  <input
                    type="text"
                    id="address"
                    v-model="customerInfo.address"
                    required
                    placeholder="Enter your full address"
                  >
                </div>

                <div class="form-group">
                  <label for="city">City <span class="required">*</span></label>
                  <input
                    type="text"
                    id="city"
                    v-model="customerInfo.city"
                    required
                    placeholder="Enter your city"
                  >
                </div>

                <div class="form-group">
                  <label for="zipCode">Postal Code <span class="required">*</span></label>
                  <input
                    type="text"
                    id="zipCode"
                    v-model="customerInfo.zipCode"
                    required
                    placeholder="Enter your postal code"
                  >
                </div>
              </div>

              <h2>Payment Method</h2>

              <div class="payment-methods">
                <div class="payment-method" :class="{ 'active': paymentMethod === 'cash' }" @click="paymentMethod = 'cash'">
                  <div class="payment-radio">
                    <div class="radio-inner" v-if="paymentMethod === 'cash'"></div>
                  </div>
                  <div class="payment-label">
                    <img src="../assets/easypaisa.png" alt="" style="width: 100px;height: 70px;">
                  </div>
                </div>

                <div class="payment-method" :class="{ 'active': paymentMethod === 'bank' }" @click="paymentMethod = 'bank'">
                  <div class="payment-radio">
                    <div class="radio-inner" v-if="paymentMethod === 'bank'"></div>
                  </div>
                  <div class="payment-label">
                    <img src="../assets/jazzcash.png" alt="" style="width: 100px;height: 70px;">
                  </div>
                </div>
              </div>

              <div class="order-summary">
                <div class="summary-row total">
                  <div class="summary-label">Total</div>
                  <div class="summary-value">Rs. {{ formatPrice(cartTotal + shippingCost) }}</div>
                </div>
              </div>

              <div class="checkout-actions">
                <router-link to="/checkout" class="btn btn-secondary" @click.native="setCheckoutStep(1)">
                  <i class="fas fa-arrow-left"></i> Back to Order Summary
                </router-link>

                <button type="submit" class="btn btn-primary">
                  Place Order <i class="fas fa-check"></i>
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState, mapGetters, mapActions } from 'vuex';

export default {
  name: 'CheckoutInformation',
  data() {
    return {
      customerInfo: {
        name: '',
        email: '',
        phone: '',
        address: '',
        city: '',
        zipCode: ''
      },
      paymentMethod: 'cash'
    };
  },
  computed: {
    ...mapState(['cart', 'checkout']),
    ...mapGetters(['cartTotal']),

    shippingCost() {
      return this.cartTotal >= 3000 ? 0 : 300;
    }
  },
  methods: {
    ...mapActions(['setCheckoutStep', 'setCustomerInfo', 'placeOrder']),

    formatPrice(price) {
      return price.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    },

    submitForm() {
      // Save customer information
      this.setCustomerInfo({
        ...this.customerInfo,
        paymentMethod: this.paymentMethod
      });

      // Place the order
      this.placeOrder();

      // Navigate to success page
      this.$router.push('/checkout/success');
    }
  },
  created() {
    // Load cart from localStorage if available
    this.$store.dispatch('loadCart');

    // If cart is empty, redirect to cart page
    if (this.cart.length === 0) {
      this.$router.push('/cart');
      return;
    }

    // Set checkout step to 2
    this.setCheckoutStep(2);

    // Load existing customer info if available
    if (this.checkout.customerInfo) {
      this.customerInfo = { ...this.checkout.customerInfo };
      if (this.checkout.customerInfo.paymentMethod) {
        this.paymentMethod = this.checkout.customerInfo.paymentMethod;
      }
    }
  }
};
</script>

<style scoped>
:root {
  --primary-color: #ff8a8a; /* 淡红色主题色 */
  --primary-hover: #ff6b6b;
  --primary-light: rgba(255, 138, 138, 0.12);
  --success-color: #67c23a;
  --text-dark: #333;
  --text-light: #777;
  --border-light: #eeeeee;
  --shadow-light: 0 4px 12px rgba(0, 0, 0, 0.05);
  --shadow-medium: 0 6px 16px rgba(0, 0, 0, 0.1);
  --transition-normal: all 0.3s ease;
}

/* ===== 全局样式 ===== */
.checkout-page {
  padding: 40px 0 80px;
  background-color: #f9f9f9;
  min-height: 100vh;
}

.section-title {
  text-align: center;
  font-size: 2.2rem;
  margin-bottom: 2rem;
  color: var(--text-dark);
  position: relative;
  letter-spacing: 0.5px;
}

.section-title::after {
  content: '';
  position: absolute;
  bottom: -10px;
  left: 50%;
  transform: translateX(-50%);
  width: 60px;
  height: 3px;
  background-color: var(--primary-color);
  border-radius: 3px;
}

/* ===== 空购物车状态 ===== */
.checkout-empty {
  text-align: center;
  padding: 60px 0;
  animation: fadeIn 0.6s ease-out;
}

.checkout-empty i {
  font-size: 5rem;
  color: #e0e0e0;
  margin-bottom: 1.5rem;
  animation: bounce 2s infinite;
}

.checkout-empty h2 {
  font-size: 2rem;
  margin-bottom: 15px;
  color: var(--text-dark);
}

.checkout-empty p {
  color: var(--text-light);
  margin-bottom: 30px;
  max-width: 500px;
  margin-left: auto;
  margin-right: auto;
  line-height: 1.6;
}

.btn {
  padding: 12px 28px;
  font-size: 1rem;
  font-weight: 600;
  border-radius: 30px;
  cursor: pointer;
  transition: var(--transition-normal);
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  border: none;
  text-decoration: none;
}

.btn-primary {
  background-color: var(--primary-color);
  color: white;
  box-shadow: 0 4px 10px rgba(255, 138, 138, 0.3);
}

.btn-primary:hover {
  background-color: var(--primary-hover);
  transform: translateY(-2px);
  box-shadow: 0 6px 14px rgba(255, 138, 138, 0.35);
}

.btn-secondary {
  background-color: #f0f0f0;
  color: var(--text-dark);
}

.checkout-container {
  max-width: 800px;
  margin: 0 auto;
  animation: slideUp 0.5s ease-out;
}

/* Progress Bar */
.checkout-progress {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 40px;
}

.progress-step {
  display: flex;
  flex-direction: column;
  align-items: center;
  flex: 0 0 auto;
  position: relative;
  z-index: 1;
  transition: var(--transition-normal);
}

.step-number {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: #f5f5f5;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  margin-bottom: 10px;
  color: var(--text-light);
  border: 2px solid transparent;
  transition: var(--transition-normal);
}

.step-label {
  font-size: 0.9rem;
  color: var(--text-light);
  font-weight: 500;
  transition: var(--transition-normal);
}

.progress-bar {
  flex-grow: 1;
  height: 3px;
  background-color: #f0f0f0;
  margin: 0 15px;
  margin-bottom: 28px;
  position: relative;
  overflow: hidden;
  transition: var(--transition-normal);
}

.progress-bar.active::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: var(--gradient-primary);
  animation: progressBarFill 0.8s ease-out forwards;
}

@keyframes progressBarFill {
  from { transform: translateX(-100%); }
  to { transform: translateX(0); }
}

.progress-step.active .step-number {
  background-image: var(--gradient-primary);
  color: white;
  border-color: var(--primary-color);
  box-shadow: var(--shadow-primary);
  transform: scale(1.05);
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

/* Main Checkout Content */
.checkout-content {
  background-color: white;
  border-radius: 12px;
  box-shadow: var(--shadow-medium);
  padding: 35px;
  animation: fadeIn 0.6s ease-out;
  transition: var(--transition-normal);
}

.checkout-content:hover {
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.08);
}

.information-form h2 {
  font-size: 1.4rem;
  margin-bottom: 25px;
  color: var(--text-dark);
  position: relative;
  padding-bottom: 12px;
  border-bottom: 1px solid var(--border-light);
}

.information-form h2::after {
  content: '';
  position: absolute;
  bottom: -1px;
  left: 0;
  width: 60px;
  height: 3px;
  background-image: var(--gradient-primary);
  border-radius: 3px;
  transition: width 0.3s ease;
}

.information-form:hover h2::after {
  width: 80px;
}

.form-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
  margin-bottom: 35px;
}

.form-group {
  display: flex;
  flex-direction: column;
  animation: fadeIn 0.6s ease-out;
}

.form-group.full-width {
  grid-column: span 2;
}

.form-group label {
  margin-bottom: 10px;
  font-weight: 500;
  color: var(--text-dark);
  transition: var(--transition-normal);
}

.required {
  color: var(--primary-color);
  margin-left: 3px;
}

.form-group input {
  padding: 14px 16px;
  border: 1px solid var(--border-light);
  border-radius: 8px;
  font-size: 1rem;
  transition: var(--transition-normal);
  background-color: #fafafa;
}

.form-group input:hover {
  border-color: #ddd;
  background-color: white;
}

.form-group input:focus {
  outline: none;
  border-color: var(--primary-color);
  background-color: white;
  box-shadow: 0 0 0 3px var(--primary-light);
}

/* Payment Methods */
.payment-methods {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
  margin-bottom: 35px;
}

.payment-method {
  display: flex;
  align-items: center;
  padding: 18px;
  border: 1px solid var(--border-light);
  border-radius: 10px;
  cursor: pointer;
  transition: var(--transition-normal);
  background-color: #fafafa;
}

.payment-method:hover {
  background-color: white;
  transform: translateY(-3px);
  box-shadow: var(--shadow-light);
  border-color: #ddd;
}

.payment-method.active {
  border-color: var(--primary-color);
  background-color: var(--primary-light);
  transform: translateY(-5px);
  box-shadow: 0 8px 20px rgba(255, 138, 138, 0.15);
}

.payment-radio {
  width: 22px;
  height: 22px;
  border-radius: 50%;
  border: 2px solid #ddd;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 15px;
  transition: var(--transition-normal);
}

.payment-method.active .payment-radio {
  border-color: var(--primary-color);
}

.radio-inner {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background-color: var(--primary-color);
  transition: transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1.275);
  transform: scale(0);
}

.payment-method.active .radio-inner {
  transform: scale(1);
}

.payment-icon {
  font-size: 1.4rem;
  color: var(--text-light);
  margin-right: 15px;
  transition: var(--transition-normal);
}

.payment-method.active .payment-icon {
  color: var(--primary-color);
}

.payment-label {
  font-weight: 500;
  color: var(--text-dark);
}

/* Order Summary */
.order-summary {
  background-color: #f9f9fb;
  border-radius: 12px;
  padding: 25px;
  margin-bottom: 35px;
  transition: var(--transition-normal);
  border: 1px solid transparent;
}

.order-summary:hover {
  border-color: var(--border-light);
  box-shadow: var(--shadow-light);
  transform: translateY(-3px);
}

.summary-row {
  display: flex;
  justify-content: space-between;
  padding: 12px 0;
  transition: var(--transition-normal);
}

.summary-row:not(:last-child) {
  border-bottom: 1px dashed rgba(0,0,0,0.07);
}

.summary-row.total {
  font-weight: 700;
  font-size: 1.3rem;
  color: var(--text-dark);
  padding-top: 15px;
  margin-top: 5px;
}

.summary-row.total .summary-value {
  color: var(--primary-color);
  background-image: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

/* Buttons */
.checkout-actions {
  display: flex;
  justify-content: space-between;
  margin-top: 15px;
}

.checkout-actions .btn {
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.checkout-actions .btn:hover {
  transform: translateY(-3px);
}

.checkout-actions .btn-primary {
  background-image: var(--gradient-primary);
  box-shadow: var(--shadow-primary);
}

.checkout-actions .btn-primary:hover {
  box-shadow: 0 8px 20px rgba(255, 107, 107, 0.4);
}

.checkout-actions .btn-secondary {
  background-color: transparent;
  color: var(--text-dark);
  border: 1px solid var(--border-light);
}

.checkout-actions .btn-secondary:hover {
  background-color: #f8f8f8;
  border-color: #ddd;
}

/* Animations */
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

@keyframes bounce {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}

/* Responsive Design */
@media (max-width: 768px) {
  .checkout-content {
    padding: 25px 20px;
  }

  .section-title {
    font-size: 1.8rem;
  }
}
</style>
