<template>
  <div class="cart-page">
    <div class="container">
      <h1 class="section-title">Your Shopping Cart</h1>

      <div class="cart-empty" v-if="cart.length === 0">
        <i class="fas fa-shopping-cart"></i>
        <h2>Your cart is empty</h2>
        <p>You haven't added any products to your cart yet.</p>
        <router-link to="/" class="btn btn-primary">Continue Shopping</router-link>
      </div>

      <div class="cart-container" v-else>
        <div class="cart-items">
          <div class="cart-item" v-for="item in cart" :key="item.id">
            <div class="item-image">
              <img :src="item.image" :alt="item.name">
            </div>

            <div class="item-details">
              <router-link :to="`/product/${item.id}`" class="item-name">{{ item.name }}</router-link>
              <div class="item-category">Category: {{ item.category }}</div>
              <div class="item-price">Rs. {{ formatPrice(item.price) }}</div>
            </div>

            <div class="item-quantity">
              <div class="quantity-selector">
                <button @click="decrementQuantity(item)" :disabled="item.quantity <= 1">
                  <i class="fas fa-minus"></i>
                </button>
                <input type="number" v-model.number="item.quantity" @change="updateQuantity(item)" min="1" max="10">
                <button @click="incrementQuantity(item)" :disabled="item.quantity >= 10">
                  <i class="fas fa-plus"></i>
                </button>
              </div>
            </div>

            <div class="item-subtotal">
              <div class="subtotal-label">Subtotal:</div>
              <div class="subtotal-value">Rs. {{ formatPrice(item.price * item.quantity) }}</div>
            </div>

            <button class="remove-item" @click="removeItem(item.id)">
              <i class="fas fa-trash-alt"></i>
            </button>
          </div>
        </div>

        <div class="cart-summary">
          <div class="cart-total">
            <h3>Order Summary</h3>

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

            <div class="cart-actions">
              <router-link to="/checkout" class="btn btn-primary btn-block">
                Proceed to Checkout
              </router-link>

              <router-link to="/" class="btn btn-secondary btn-block">
                Continue Shopping
              </router-link>
            </div>
          </div>

          <div class="cart-note">
            <p><i class="fas fa-info-circle"></i> Maximum order value: Rs. 50,000</p>
            <p><i class="fas fa-truck"></i> Free shipping on orders over Rs. 3,000</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState, mapGetters, mapActions } from 'vuex';

export default {
  name: 'Cart',
  computed: {
    ...mapState(['cart']),
    ...mapGetters(['cartTotal']),

    shippingCost() {
      return this.cartTotal >= 3000 ? 0 : 300;
    }
  },
  methods: {
    ...mapActions(['removeFromCart', 'updateCartQuantity']),

    formatPrice(price) {
      return price.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    },

    incrementQuantity(item) {
      if (item.quantity < 10) {
        this.updateQuantity({
          ...item,
          quantity: item.quantity + 1
        });
      }
    },

    decrementQuantity(item) {
      if (item.quantity > 1) {
        this.updateQuantity({
          ...item,
          quantity: item.quantity - 1
        });
      }
    },

    updateQuantity(item) {
      // Ensure quantity is at least 1
      const quantity = Math.max(1, Math.min(10, item.quantity));

      this.updateCartQuantity({
        productId: item.id,
        quantity
      });
    },

    removeItem(productId) {
      if (confirm('Are you sure you want to remove this item from your cart?')) {
        this.removeFromCart(productId);
      }
    }
  },
  created() {
    // Load cart from localStorage if available
    this.$store.dispatch('loadCart');
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

.cart-page {
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
.cart-empty {
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

.cart-empty i {
  font-size: 5rem;
  color: #e0e0e0;
  margin-bottom: 1.5rem;
  animation: float 3s ease-in-out infinite;
}

.cart-empty h2 {
  font-size: 2rem;
  margin-bottom: 15px;
  color: var(--text-dark);
  font-weight: 600;
}

.cart-empty p {
  color: var(--text-light);
  margin-bottom: 25px;
  font-size: 1.1rem;
  max-width: 400px;
  margin-left: auto;
  margin-right: auto;
}

.cart-empty .btn {
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

.cart-empty .btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 20px rgba(255, 107, 107, 0.4);
}

.cart-empty .btn i {
  font-size: 1rem;
  margin-right: 5px;
  animation: none;
  color: white;
}

/* 购物车容器 */
.cart-container {
  display: grid;
  grid-template-columns: 2fr 1fr;
  gap: 30px;
  margin-top: 30px;
  opacity: 0;
  transform: translateY(20px);
  animation: slideUp 0.6s ease-out forwards;
  animation-delay: 0.2s;
}

/* 购物车商品列表 */
.cart-items {
  background-color: white;
  border-radius: 16px;
  box-shadow: var(--shadow-medium);
  overflow: hidden;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.cart-item {
  display: grid;
  grid-template-columns: 100px 1fr auto auto auto;
  gap: 20px;
  padding: 25px;
  border-bottom: 1px solid var(--border-light);
  align-items: center;
  position: relative;
  transition: var(--transition-normal);
  overflow: hidden;
}

.cart-item::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 3px;
  background-image: var(--gradient-primary);
  transform: scaleY(0);
  transition: transform 0.3s ease;
}

.cart-item:hover {
  background-color: rgba(255, 138, 138, 0.02);
}

.cart-item:hover::before {
  transform: scaleY(1);
}

.cart-item:last-child {
  border-bottom: none;
}

/* 商品图片 */
.item-image {
  width: 100px;
  height: 100px;
  overflow: hidden;
  border-radius: 8px;
  box-shadow: var(--shadow-light);
  transition: transform 0.5s cubic-bezier(0.165, 0.84, 0.44, 1);
}

.item-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.8s cubic-bezier(0.165, 0.84, 0.44, 1);
}

.cart-item:hover .item-image {
  transform: translateY(-5px);
}

.cart-item:hover .item-image img {
  transform: scale(1.1);
}

/* 商品详情 */
.item-name {
  font-weight: 600;
  color: var(--text-dark);
  text-decoration: none;
  display: block;
  margin-bottom: 8px;
  position: relative;
  overflow: hidden;
  transition: color 0.3s ease;
}

.item-name::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 1px;
  background-image: var(--gradient-primary);
  transform: translateX(-100%);
  transition: transform 0.3s ease;
}

.item-name:hover {
  color: var(--primary-color);
}

.item-name:hover::after {
  transform: translateX(0);
}

.item-category {
  color: var(--text-light);
  font-size: 0.9rem;
  margin-bottom: 8px;
}

.item-price {
  font-weight: 700;
  background-image: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  color: transparent;
}

/* 数量选择器 */
.quantity-selector {
  display: flex;
  border: 1px solid var(--border-light);
  border-radius: 8px;
  overflow: hidden;
  width: 120px;
  box-shadow: var(--shadow-light);
  transition: box-shadow 0.3s ease;
}

.cart-item:hover .quantity-selector {
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.07);
}

.quantity-selector button {
  width: 36px;
  height: 36px;
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
  width: 48px;
  text-align: center;
  border: none;
  border-left: 1px solid var(--border-light);
  border-right: 1px solid var(--border-light);
  font-size: 0.95rem;
  color: var(--text-dark);
  font-weight: 600;
}

/* 小计金额 */
.item-subtotal {
  text-align: right;
  transition: transform 0.3s ease;
}

.cart-item:hover .item-subtotal {
  transform: scale(1.05);
}

.subtotal-label {
  font-size: 0.9rem;
  color: var(--text-light);
  margin-bottom: 5px;
}

.subtotal-value {
  font-weight: 700;
  background-image: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  color: transparent;
  font-size: 1.1rem;
}

/* 删除按钮 */
.remove-item {
  background: none;
  border: none;
  color: var(--text-light);
  font-size: 1.1rem;
  cursor: pointer;
  transition: all 0.3s ease;
  width: 36px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
}

.remove-item:hover {
  color: #ff4747;
  background-color: rgba(255, 71, 71, 0.1);
  transform: rotate(8deg);
}

/* 订单摘要部分 */
.cart-summary {
  position: sticky;
  top: 100px;
  transform: translateY(20px);
  opacity: 0;
  animation: slideUp 0.6s ease-out forwards;
  animation-delay: 0.4s;
}

.cart-total {
  background-color: white;
  border-radius: 16px;
  box-shadow: var(--shadow-medium);
  padding: 25px;
  margin-bottom: 25px;
  position: relative;
  overflow: hidden;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.cart-total:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
}

.cart-total::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 4px;
  background-image: var(--gradient-primary);
}

.cart-total h3 {
  margin-bottom: 25px;
  text-align: center;
  position: relative;
  padding-bottom: 12px;
  color: var(--text-dark);
  font-size: 1.4rem;
}

.cart-total h3:after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 50px;
  height: 3px;
  background-image: var(--gradient-primary);
  border-radius: 2px;
  transition: width 0.4s cubic-bezier(0.165, 0.84, 0.44, 1);
}

.cart-total:hover h3:after {
  width: 80px;
}

.summary-row {
  display: flex;
  justify-content: space-between;
  margin-bottom: 15px;
  padding-bottom: 15px;
  border-bottom: 1px solid var(--border-light);
  transition: transform 0.3s ease;
}

.summary-row:hover {
  transform: translateX(5px);
}

.summary-row .summary-label {
  color: var(--text-light);
}

.summary-row .summary-value {
  font-weight: 600;
  color: var(--text-dark);
}

.summary-row:last-of-type {
  border-bottom: none;
}

.summary-row.total {
  font-weight: 700;
  font-size: 1.2rem;
  color: var(--text-dark);
  margin-top: 15px;
  margin-bottom: 25px;
  border-top: 2px solid var(--border-light);
  border-bottom: none;
  padding-top: 15px;
}

.summary-row.total .summary-value {
  background-image: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  color: transparent;
}

/* 购物车按钮 */
.cart-actions {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.cart-actions .btn {
  padding: 12px 20px;
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

.cart-actions .btn-primary {
  background-image: var(--gradient-primary);
  color: white;
  box-shadow: var(--shadow-primary);
}

.cart-actions .btn-primary:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 20px rgba(255, 107, 107, 0.4);
}

.cart-actions .btn-secondary {
  background-color: white;
  color: var(--text-dark);
  border: 1px solid var(--border-light);
}

.cart-actions .btn-secondary:hover {
  background-color: #f8f9fa;
  color: var(--primary-color);
  transform: translateY(-3px);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
}

.cart-actions .btn i {
  font-size: 0.9rem;
  transition: transform 0.3s ease;
}

.cart-actions .btn:hover i {
  transform: translateX(3px);
}

/* 购物车备注 */
.cart-note {
  background: linear-gradient(145deg, #ffffff, #f8f9fa);
  border-radius: 16px;
  padding: 20px;
  box-shadow: var(--shadow-light);
  border-left: 3px solid var(--primary-color);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.cart-note:hover {
  transform: translateY(-5px);
  box-shadow: var(--shadow-medium);
}

.cart-note p {
  margin-bottom: 12px;
  font-size: 0.95rem;
  display: flex;
  align-items: center;
  color: var(--text-light);
  transition: transform 0.3s ease;
}

.cart-note p:hover {
  transform: translateX(5px);
  color: var(--text-dark);
}

.cart-note p:last-child {
  margin-bottom: 0;
}

.cart-note i {
  background-image: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  color: transparent;
  margin-right: 10px;
  font-size: 1.1rem;
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

/* 响应式布局 */
@media (max-width: 992px) {
  .cart-container {
    grid-template-columns: 1fr;
    gap: 25px;
  }

  .cart-item {
    grid-template-columns: 80px 1fr;
    gap: 15px;
    padding: 20px;
  }

  .item-image {
    width: 80px;
    height: 80px;
    grid-row: span 2;
  }

  .item-quantity {
    grid-column: 1;
    grid-row: 3;
    margin-top: 10px;
  }

  .item-subtotal {
    grid-column: 2;
    grid-row: 3;
    text-align: left;
    margin-top: 10px;
  }

  .remove-item {
    position: absolute;
    top: 15px;
    right: 15px;
    background-color: rgba(255, 255, 255, 0.8);
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  }
  
  .section-title {
    font-size: 1.8rem;
  }
}

@media (max-width: 576px) {
  .cart-item {
    position: relative;
    padding-right: 45px;
  }
  
  .cart-page {
    padding: 20px 0 50px;
  }
  
  .cart-empty {
    padding: 50px 20px;
  }
  
  .cart-empty h2 {
    font-size: 1.5rem;
  }
  
  .cart-empty p {
    font-size: 1rem;
  }
  
  .quantity-selector {
    width: 110px;
  }
  
  .quantity-selector input {
    width: 38px;
  }
  
  .cart-total {
    padding: 20px;
  }
}
</style>
