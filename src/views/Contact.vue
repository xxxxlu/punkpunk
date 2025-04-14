<template>
  <div class="contact-page">
    <div class="container">
      <h1 class="section-title">Contact Us</h1>

      <div class="contact-container">
        <div class="contact-info">
          <h2>Get in Touch</h2>
          <p>Have questions about our products or your order? We're here to help! Use the contact information below or fill out the form to reach our customer service team.</p>

          <div class="info-items">

            <div class="info-item">
              <div class="info-icon">
                <i class="fas fa-envelope"></i>
              </div>
              <div class="info-content">
                <h3>Email</h3>
                <p>punkpunk@punk.shop</p>
              </div>
            </div>

            <div class="info-item">
              <div class="info-icon">
                <i class="fas fa-clock"></i>
              </div>
              <div class="info-content">
                <h3>Business Hours</h3>
                <p>Monday - Friday: 9:00 AM - 6:00 PM</p>
                <p>Saturday: 10:00 AM - 4:00 PM</p>
                <p>Sunday: Closed</p>
              </div>
            </div>
          </div>

          <div class="social-links">
            <h3>Follow Us</h3>
            <div class="social-icons">
              <a href="#" class="social-icon"><i class="fab fa-facebook-f"></i></a>
              <a href="#" class="social-icon"><i class="fab fa-twitter"></i></a>
              <a href="#" class="social-icon"><i class="fab fa-instagram"></i></a>
              <a href="#" class="social-icon"><i class="fab fa-youtube"></i></a>
            </div>
          </div>
        </div>

        <div class="contact-form">
          <h2>Send Us a Message</h2>

          <form @submit.prevent="submitForm">
            <div class="form-group">
              <label for="name">Name <span class="required">*</span></label>
              <input
                type="text"
                id="name"
                v-model="form.name"
                required
                placeholder="Enter your name"
              >
            </div>

            <div class="form-group">
              <label for="email">Email <span class="required">*</span></label>
              <input
                type="email"
                id="email"
                v-model="form.email"
                required
                placeholder="Enter your email"
              >
            </div>

            <div class="form-group">
              <label for="subject">Subject <span class="required">*</span></label>
              <input
                type="text"
                id="subject"
                v-model="form.subject"
                required
                placeholder="Enter message subject"
              >
            </div>

            <div class="form-group">
              <label for="message">Message <span class="required">*</span></label>
              <textarea
                id="message"
                v-model="form.message"
                required
                placeholder="Enter your message"
                rows="6"
              ></textarea>
            </div>

            <button type="submit" class="btn btn-primary btn-block">
              Send Message <i class="fas fa-paper-plane"></i>
            </button>
          </form>

          <div class="form-success" v-if="formSubmitted">
            <i class="fas fa-check-circle"></i>
            <h3>Thank You!</h3>
            <p>Your message has been sent successfully. We'll respond to you as soon as possible.</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Contact',
  data() {
    return {
      form: {
        name: '',
        email: '',
        subject: '',
        message: ''
      },
      formSubmitted: false
    };
  },
  methods: {
    submitForm() {
      // In a real app, you would send this data to a server
      // For this demo, we'll just show a success message
      this.formSubmitted = true;

      // Reset form for demo purposes
      setTimeout(() => {
        this.formSubmitted = false;
        this.form = {
          name: '',
          email: '',
          subject: '',
          message: ''
        };
      }, 5000);
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

.contact-page {
  padding: 40px 0 70px;
  animation: fadeIn 0.8s ease-out;
}

.contact-container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 40px;
  animation: slideUp 0.5s ease-out;
}

/* Contact Info Section */
.contact-info {
  background-color: white;
  border-radius: 12px;
  box-shadow: var(--shadow-medium);
  padding: 35px;
  transition: var(--transition-normal);
  animation: fadeIn 0.6s ease-out;
}

.contact-info:hover {
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.08);
  transform: translateY(-5px);
}

.contact-info h2,
.contact-form h2 {
  font-size: 1.6rem;
  margin-bottom: 22px;
  color: var(--text-dark);
  position: relative;
  padding-bottom: 12px;
  font-weight: 600;
}

.contact-info h2:after,
.contact-form h2:after {
  content: '';
  position: absolute;
  left: 0;
  bottom: 0;
  width: 60px;
  height: 3px;
  background-image: var(--gradient-primary);
  border-radius: 3px;
  transition: width 0.3s ease;
}

.contact-info:hover h2:after,
.contact-form:hover h2:after {
  width: 80px;
}

.contact-info > p {
  color: var(--text-light);
  margin-bottom: 30px;
  line-height: 1.7;
  font-size: 1.05rem;
}

.info-items {
  margin-bottom: 35px;
}

.info-item {
  display: flex;
  margin-bottom: 25px;
  transition: var(--transition-normal);
  animation: slideUp 0.5s ease-out;
  animation-fill-mode: both;
}

.info-item:nth-child(1) { animation-delay: 0.1s; }
.info-item:nth-child(2) { animation-delay: 0.2s; }
.info-item:nth-child(3) { animation-delay: 0.3s; }
.info-item:nth-child(4) { animation-delay: 0.4s; }

.info-item:hover {
  transform: translateX(5px);
}

.info-item:last-child {
  margin-bottom: 0;
}

.info-icon {
  width: 48px;
  height: 48px;
  background-image: var(--gradient-primary);
  color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.2rem;
  margin-right: 18px;
  flex-shrink: 0;
  box-shadow: var(--shadow-primary);
  transition: var(--transition-normal);
}

.info-item:hover .info-icon {
  transform: scale(1.1);
  box-shadow: 0 8px 20px rgba(255, 107, 107, 0.4);
}

.info-content h3 {
  font-size: 1.2rem;
  margin-bottom: 8px;
  color: var(--text-dark);
  font-weight: 600;
  transition: var(--transition-normal);
}

.info-item:hover .info-content h3 {
  color: var(--primary-color);
}

.info-content p {
  color: var(--text-light);
  margin-bottom: 3px;
  line-height: 1.5;
}

.social-links h3 {
  font-size: 1.2rem;
  margin-bottom: 18px;
  color: var(--text-dark);
  font-weight: 600;
}

.social-icons {
  display: flex;
  gap: 15px;
}

.social-icon {
  width: 45px;
  height: 45px;
  background-color: #f8f8f8;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--text-light);
  font-size: 1.2rem;
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  position: relative;
  overflow: hidden;
  z-index: 1;
}

.social-icon::before {
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

.social-icon:hover {
  color: white;
  transform: translateY(-5px) scale(1.1);
  box-shadow: var(--shadow-primary);
}

.social-icon:hover::before {
  opacity: 1;
  transform: scale(1);
}

/* Contact Form Section */
.contact-form {
  background-color: white;
  border-radius: 12px;
  box-shadow: var(--shadow-medium);
  padding: 35px;
  position: relative;
  transition: var(--transition-normal);
  animation: fadeIn 0.6s ease-out 0.2s;
  animation-fill-mode: both;
}

.contact-form:hover {
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.08);
  transform: translateY(-5px);
}

.form-group {
  margin-bottom: 24px;
  animation: slideUp 0.5s ease-out;
  animation-fill-mode: both;
}

.form-group:nth-child(1) { animation-delay: 0.3s; }
.form-group:nth-child(2) { animation-delay: 0.4s; }
.form-group:nth-child(3) { animation-delay: 0.5s; }
.form-group:nth-child(4) { animation-delay: 0.6s; }

.form-group label {
  display: block;
  margin-bottom: 10px;
  font-weight: 500;
  color: var(--text-dark);
  transition: var(--transition-normal);
}

.form-group:hover label {
  transform: translateX(5px);
}

.required {
  color: var(--primary-color);
  margin-left: 3px;
}

.form-group input,
.form-group textarea {
  width: 100%;
  padding: 14px 16px;
  border: 1px solid var(--border-light);
  border-radius: 8px;
  font-size: 1rem;
  font-family: inherit;
  transition: var(--transition-normal);
  background-color: #fafafa;
}

.form-group input:hover,
.form-group textarea:hover {
  border-color: #ddd;
  background-color: white;
}

.form-group input:focus,
.form-group textarea:focus {
  outline: none;
  border-color: var(--primary-color);
  background-color: white;
  box-shadow: 0 0 0 3px var(--primary-light);
}

.form-group textarea {
  resize: vertical;
}

.btn-block {
  width: 100%;
  padding: 14px;
  font-size: 1.1rem;
  background-image: var(--gradient-primary);
  box-shadow: var(--shadow-primary);
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  border: none;
  border-radius: var(--border-radius-full);
  font-weight: 600;
  color: white;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 8px;
  animation: fadeIn 0.6s ease-out 0.7s;
  animation-fill-mode: both;
}

.btn-block:hover {
  transform: translateY(-5px) scale(1.02);
  box-shadow: 0 10px 20px rgba(255, 107, 107, 0.4);
}

.btn-block i {
  margin-left: 5px;
  font-size: 0.9rem;
  transition: transform 0.3s ease;
}

.btn-block:hover i {
  transform: translateX(5px);
}

/* Form Success Message */
.form-success {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(255, 255, 255, 0.98);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border-radius: 12px;
  padding: 35px;
  animation: fadeIn 0.5s ease-out;
}

.form-success i {
  font-size: 4.5rem;
  color: var(--success-color);
  margin-bottom: 25px;
  animation: bounce 1.5s ease infinite;
  background-image: var(--success-gradient);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.form-success h3 {
  font-size: 2rem;
  margin-bottom: 15px;
  color: var(--text-dark);
  font-weight: 700;
}

.form-success p {
  color: var(--text-light);
  text-align: center;
  max-width: 500px;
  font-size: 1.1rem;
  line-height: 1.6;
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
@media (max-width: 992px) {
  .contact-container {
    grid-template-columns: 1fr;
  }
  
  .contact-info {
    margin-bottom: 20px;
  }
}

@media (max-width: 768px) {
  .contact-info, .contact-form {
    padding: 25px 20px;
  }
  
  .info-icon {
    width: 40px;
    height: 40px;
    font-size: 1rem;
  }
  
  .contact-info h2, .contact-form h2 {
    font-size: 1.4rem;
  }
  
  .btn-block {
    padding: 12px;
  }
}</style>
