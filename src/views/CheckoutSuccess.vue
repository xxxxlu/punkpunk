<template>
  <div class="checkout-page">
    <div class="container">
      <h1 class="section-title">Checkout</h1>

      <div class="checkout-container">
        <!-- Checkout Steps Progress -->
        <div class="checkout-progress">
          <div class="progress-step completed">
            <div class="step-number">1</div>
            <div class="step-label">Products</div>
          </div>
          <div class="progress-bar active"></div>
          <div class="progress-step completed">
            <div class="step-number">2</div>
            <div class="step-label">Information</div>
          </div>
          <div class="progress-bar active"></div>
          <div class="progress-step active">
            <div class="step-number">3</div>
            <div class="step-label">Confirmation</div>
          </div>
        </div>

        <!-- Order Success (Step 3) -->
        <div class="checkout-content">
          <div class="success-message">
            <div class="success-icon">
              <i class="fas fa-check-circle"></i>
            </div>

            <h2>Thank You for Your Order!</h2>
            <p>Your order has been placed successfully. You will receive a confirmation email shortly.</p>

            <div class="order-details">
              <h3>Order Details</h3>

              <div class="detail-row">
                <div class="detail-label">Order Number:</div>
                <div class="detail-value">{{ orderNumber }}</div>
              </div>

              <div class="detail-row">
                <div class="detail-label">Date:</div>
                <div class="detail-value">{{ currentDate }}</div>
              </div>

              <div class="detail-row">
                <div class="detail-label">Payment Method:</div>
                <div class="detail-value">{{ paymentMethodLabel }}</div>
              </div>

              <div class="detail-row">
                <div class="detail-label">Total Amount:</div>
                <div class="detail-value">Rs. {{ orderTotal }}</div>
              </div>
            </div>

            <div class="customer-info">
              <h3>Shipping Information</h3>

              <div class="info-group" v-if="checkout.customerInfo">
                <div class="info-row">
                  <div class="info-label">Name:</div>
                  <div class="info-value">{{ checkout.customerInfo.name }}</div>
                </div>

                <div class="info-row">
                  <div class="info-label">Email:</div>
                  <div class="info-value">{{ checkout.customerInfo.email }}</div>
                </div>

                <div class="info-row">
                  <div class="info-label">Phone:</div>
                  <div class="info-value">{{ checkout.customerInfo.phone }}</div>
                </div>

                <div class="info-row">
                  <div class="info-label">Address:</div>
                  <div class="info-value">{{ checkout.customerInfo.address }}</div>
                </div>

                <div class="info-row">
                  <div class="info-label">City:</div>
                  <div class="info-value">{{ checkout.customerInfo.city }}</div>
                </div>

                <div class="info-row">
                  <div class="info-label">Postal Code:</div>
                  <div class="info-value">{{ checkout.customerInfo.zipCode }}</div>
                </div>
              </div>
            </div>

            <div class="success-actions">
              <router-link to="/" class="btn btn-primary">
                Continue Shopping
              </router-link>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex';

export default {
  name: 'CheckoutSuccess',
  computed: {
    ...mapState(['checkout']),

    orderNumber() {
      // Generate a random order number
      return 'ORD-' + Math.floor(100000 + Math.random() * 900000);
    },

    currentDate() {
      const date = new Date();
      return date.toLocaleDateString('en-US', {
        year: 'numeric',
        month: 'long',
        day: 'numeric'
      });
    },

    paymentMethodLabel() {
      if (!this.checkout.customerInfo || !this.checkout.customerInfo.paymentMethod) {
        return 'Cash on Delivery';
      }

      return this.checkout.customerInfo.paymentMethod === 'cash'
        ? 'Cash on Delivery'
        : 'Bank Transfer';
    },

    orderTotal() {
      // In a real app, you would get this from the store
      // Here we're generating a random number between 1000 and 10000
      const total = Math.floor(1000 + Math.random() * 9000);
      return total.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    }
  },
  created() {
    // Check if we're on step 3, if not redirect to checkout
    if (this.checkout.step !== 3) {
      this.$router.push('/checkout');
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
  --success-gradient: linear-gradient(135deg, #67c23a 0%, #52b625 100%);
  --success-shadow: 0 5px 15px rgba(103, 194, 58, 0.3);
}

.checkout-page {
  padding: 40px 0 70px;
  animation: fadeIn 0.8s ease-out;
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

/* Success Content */
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

.success-message {
  text-align: center;
  animation: slideUp 0.6s ease-out;
}

.success-icon {
  font-size: 4.5rem;
  color: var(--success-color);
  margin-bottom: 25px;
  animation: bounce 1.5s ease infinite;
  background-image: var(--success-gradient);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.success-message h2 {
  font-size: 2rem;
  margin-bottom: 18px;
  color: var(--text-dark);
  font-weight: 700;
  background-image: linear-gradient(135deg, #333 0%, #555 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.success-message p {
  color: var(--text-light);
  max-width: 600px;
  margin: 0 auto 35px;
  font-size: 1.1rem;
  line-height: 1.6;
}

.order-details, .customer-info {
  background-color: #f9f9fb;
  border-radius: 12px;
  padding: 25px;
  margin-bottom: 30px;
  text-align: left;
  border: 1px solid transparent;
  transition: var(--transition-normal);
}

.order-details:hover, .customer-info:hover {
  border-color: var(--border-light);
  box-shadow: var(--shadow-light);
  transform: translateY(-5px);
}

.order-details h3, .customer-info h3 {
  font-size: 1.4rem;
  margin-bottom: 18px;
  padding-bottom: 12px;
  border-bottom: 1px solid var(--border-light);
  color: var(--text-dark);
  position: relative;
}

.order-details h3::after, .customer-info h3::after {
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

.order-details:hover h3::after, .customer-info:hover h3::after {
  width: 80px;
}

.detail-row, .info-row {
  display: flex;
  margin-bottom: 12px;
  transition: var(--transition-normal);
}

.detail-row:hover, .info-row:hover {
  transform: translateX(5px);
}

.detail-row:last-child, .info-row:last-child {
  margin-bottom: 0;
}

.detail-label, .info-label {
  flex: 0 0 150px;
  font-weight: 600;
  color: var(--text-dark);
}

.detail-value, .info-value {
  flex: 1;
  color: var(--text-dark);
  font-weight: 500;
}

.detail-row:nth-child(4) .detail-value {
  color: var(--primary-color);
  background-image: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  font-weight: 700;
}

.success-actions {
  margin-top: 40px;
}

.success-actions .btn {
  padding: 14px 32px;
  font-size: 1.1rem;
  background-image: var(--gradient-primary);
  box-shadow: var(--shadow-primary);
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.success-actions .btn:hover {
  transform: translateY(-5px) scale(1.05);
  box-shadow: 0 10px 20px rgba(255, 107, 107, 0.4);
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

  .step-label {
    font-size: 0.8rem;
  }

  .progress-bar {
    margin: 0 10px;
    margin-bottom: 28px;
  }

  .detail-row, .info-row {
    flex-direction: column;
  }

  .detail-label, .info-label {
    margin-bottom: 5px;
  }
  
  .success-message h2 {
    font-size: 1.6rem;
  }
  
  .success-icon {
    font-size: 3.5rem;
  }
  
  .success-actions .btn {
    width: 100%;
    padding: 12px 20px;
  }
}</style>
