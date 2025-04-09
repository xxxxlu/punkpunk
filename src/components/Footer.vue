<template>
  <footer class="footer">
    <div class="container">
      <div class="footer-content">
        <div class="footer-section">
          <h3>PakistanStore</h3>
          <p>Your trusted online marketplace for authentic Pakistani products.</p>
          <div class="social-links">
            <a href="#" class="social-link"><i class="fab fa-facebook-f"></i></a>
            <a href="#" class="social-link"><i class="fab fa-twitter"></i></a>
            <a href="#" class="social-link"><i class="fab fa-instagram"></i></a>
            <a href="#" class="social-link"><i class="fab fa-youtube"></i></a>
          </div>
        </div>

        <div class="footer-section">
          <h3>Quick Links</h3>
          <ul class="footer-links">
            <li><router-link to="/">Home</router-link></li>
            <li><router-link to="/contact">Contact Us</router-link></li>
          </ul>
        </div>

        <div class="footer-section">
          <h3>Customer Service</h3>
          <ul class="footer-links">
            <li><a href="#">Order Tracking</a></li>
            <li><a href="#">Shipping Policy</a></li>
            <li><a href="#">Returns & Exchanges</a></li>
            <li><a href="#">FAQs</a></li>
          </ul>
        </div>

        <div class="footer-section">
          <h3>Contact Info</h3>
          <ul class="contact-info">
            <li><i class="fas fa-map-marker-alt"></i> 123 Main Street, Karachi, Pakistan</li>
            <li><i class="fas fa-envelope"></i> punkpunks@punk.shop</li>
          </ul>
          <div class="payment-methods">
            <img src="../assets/easypaisa.png" alt="" style="width: 100px;height: 70px;">
            <img src="../assets/jazzcash.png" alt="" style="width: 100px;height: 70px;">
          </div>
        </div>
      </div>

      <div class="footer-bottom">
        <p>&copy; Copyright © {{ currentYear }}  | Powered by Data Network Solutions (SMC-Private) Limited.</p>
      </div>
    </div>
  </footer>
</template>

<script>
export default {
  name: 'Footer',
  computed: {
    currentYear() {
      return new Date().getFullYear();
    }
  }
};
</script>

<style scoped>
.footer {
  background-image: var(--gradient-dark);
  color: #f8f9fa;
  padding: 70px 0 25px;
  margin-top: auto;
  position: relative;
  overflow: hidden;
  box-shadow: 0 -5px 20px rgba(0, 0, 0, 0.1);
}

.footer::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 5px;
  background-image: var(--gradient-primary);
}

/* 添加几个装饰性元素 */
.footer::after {
  content: '';
  position: absolute;
  top: 0;
  right: 0;
  width: 40%;
  height: 100%;
  background-image: radial-gradient(circle at center, rgba(255, 107, 107, 0.05) 0%, transparent 70%);
  z-index: 0;
  pointer-events: none;
}

.footer-content {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  gap: 40px;
  margin-bottom: 40px;
  position: relative;
  z-index: 1;
}

.footer-section {
  animation: fadeIn 0.5s ease forwards;
  opacity: 0;
  animation-delay: calc(var(--delay, 0) * 0.1s);
}

.footer-section:nth-child(1) { --delay: 1; }
.footer-section:nth-child(2) { --delay: 2; }
.footer-section:nth-child(3) { --delay: 3; }
.footer-section:nth-child(4) { --delay: 4; }

.footer-section h3 {
  color: #fff;
  font-size: 1.3rem;
  margin-bottom: 25px;
  position: relative;
  padding-bottom: 12px;
  font-weight: 600;
}

.footer-section h3:after {
  content: '';
  position: absolute;
  left: 0;
  bottom: 0;
  width: 50px;
  height: 3px;
  background-image: var(--gradient-primary);
  border-radius: var(--border-radius-full);
  transition: width 0.3s ease;
}

.footer-section:hover h3:after {
  width: 70px;
}

.footer-section p {
  color: rgba(255, 255, 255, 0.7);
  margin-bottom: 20px;
  line-height: 1.7;
  font-size: 0.95rem;
}

.social-links {
  display: flex;
  gap: 15px;
  margin-top: 20px;
}

.social-link {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  background-color: rgba(255, 255, 255, 0.1);
  color: #f8f9fa;
  border-radius: 50%;
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  position: relative;
  overflow: hidden;
  z-index: 1;
}

.social-link::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: var(--gradient-primary);
  opacity: 0;
  z-index: -1;
  transition: opacity 0.3s ease;
  transform: scale(0);
  border-radius: 50%;
}

.social-link:hover {
  color: white;
  transform: translateY(-3px) scale(1.1);
  box-shadow: 0 5px 15px rgba(255, 107, 107, 0.4);
}

.social-link:hover::before {
  opacity: 1;
  transform: scale(1);
}

.footer-links {
  list-style: none;
  padding: 0;
}

.footer-links li {
  margin-bottom: 12px;
  transition: transform 0.3s ease;
}

.footer-links li:hover {
  transform: translateX(5px);
}

.footer-links a {
  color: rgba(255, 255, 255, 0.7);
  text-decoration: none;
  transition: all 0.3s ease;
  display: block;
  padding: 3px 0;
  position: relative;
}

.footer-links a::before {
  content: '›';
  margin-right: 8px;
  color: var(--color-primary);
  font-size: 1.2em;
  opacity: 0;
  transform: translateX(-10px);
  transition: all 0.3s ease;
  display: inline-block;
}

.footer-links a:hover {
  color: white;
  padding-left: 5px;
}

.footer-links a:hover::before {
  opacity: 1;
  transform: translateX(0);
}

.contact-info {
  list-style: none;
  padding: 0;
}

.contact-info li {
  margin-bottom: 15px;
  display: flex;
  align-items: flex-start;
  color: rgba(255, 255, 255, 0.7);
  transition: all 0.3s ease;
}

.contact-info li:hover {
  color: white;
}

.contact-info i {
  margin-right: 12px;
  background-image: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  color: transparent;
  font-size: 1.1rem;
  width: 20px;
  display: inline-block;
  text-align: center;
}

.payment-methods {
  margin-top: 25px;
  display: flex;
  gap: 15px;
}

.payment-methods img {
  border-radius: var(--border-radius-sm);
  transition: all 0.3s ease;
  filter: grayscale(30%);
  opacity: 0.8;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
}

.payment-methods img:hover {
  transform: translateY(-5px);
  filter: grayscale(0%);
  opacity: 1;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
}

.footer-bottom {
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  padding-top: 25px;
  text-align: center;
  color: rgba(255, 255, 255, 0.6);
  font-size: 0.9rem;
  position: relative;
  z-index: 1;
}

.footer-bottom p {
  transition: all 0.3s ease;
}

.footer-bottom p:hover {
  color: white;
}

@media (max-width: 768px) {
  .footer {
    padding: 50px 0 20px;
  }
  
  .footer-content {
    grid-template-columns: 1fr;
    gap: 40px;
  }
  
  .footer-section h3 {
    font-size: 1.2rem;
  }
  
  .social-link {
    width: 38px;
    height: 38px;
  }
}
</style>
