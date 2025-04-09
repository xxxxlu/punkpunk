<template>
  <header class="header">
    <div class="container">
      <div class="header-inner">
        <div class="logo">
          <router-link to="/">
            <h1>punk<span>punk</span></h1>
          </router-link>
        </div>

        <nav class="nav">
          <ul class="nav-links">
            <li><router-link to="/">Home</router-link></li>
            <li class="dropdown">
              <a href="#" @click.prevent>Categories</a>
              <div class="dropdown-content">
                <router-link to="/?category=Electronics">Electronics</router-link>
                <router-link to="/?category=Clothing">Clothing</router-link>
                <router-link to="/?category=Footwear">Footwear</router-link>
                <router-link to="/?category=Kitchen">Kitchen</router-link>
                <router-link to="/?category=Home">Home</router-link>
                <router-link to="/?category=Home Decor">Home Decor</router-link>
                <router-link to="/?category=Grocery">Grocery</router-link>
                <router-link to="/?category=Beauty">Beauty</router-link>
                <router-link to="/?category=Accessories">Accessories</router-link>
              </div>
            </li>
            <li><router-link to="/contact">Contact Us</router-link></li>
          </ul>
        </nav>

        <div class="header-actions">
          <div class="search">
            <input type="text" placeholder="Search products..." v-model="searchQuery" @keyup.enter="handleSearch">
            <button @click="handleSearch"><i class="fas fa-search"></i></button>
          </div>

          <div class="cart-icon">
            <router-link to="/cart">
              <i class="fas fa-shopping-cart"></i>
              <span class="cart-count" v-if="cartItemCount > 0">{{ cartItemCount }}</span>
            </router-link>
          </div>
        </div>

        <div class="mobile-toggle" @click="toggleMobileMenu">
          <i class="fas fa-bars"></i>
        </div>
      </div>
    </div>

    <div class="mobile-menu" :class="{ 'active': mobileMenuActive }">
      <ul>
        <li><router-link to="/" @click="closeMobileMenu">Home</router-link></li>
        <li><router-link to="/?category=Electronics" @click="closeMobileMenu">Electronics</router-link></li>
        <li><router-link to="/?category=Clothing" @click="closeMobileMenu">Clothing</router-link></li>
        <li><router-link to="/?category=Footwear" @click="closeMobileMenu">Footwear</router-link></li>
        <li><router-link to="/?category=Kitchen" @click="closeMobileMenu">Kitchen</router-link></li>
        <li><router-link to="/?category=Home" @click="closeMobileMenu">Home</router-link></li>
        <li><router-link to="/?category=Home Decor" @click="closeMobileMenu">Home Decor</router-link></li>
        <li><router-link to="/?category=Grocery" @click="closeMobileMenu">Grocery</router-link></li>
        <li><router-link to="/?category=Beauty" @click="closeMobileMenu">Beauty</router-link></li>
        <li><router-link to="/?category=Accessories" @click="closeMobileMenu">Accessories</router-link></li>
        <li><router-link to="/contact" @click="closeMobileMenu">Contact Us</router-link></li>
        <li><router-link to="/cart" @click="closeMobileMenu">Cart ({{ cartItemCount }})</router-link></li>
      </ul>
    </div>
  </header>
</template>

<script>
import { mapGetters } from 'vuex';

export default {
  name: 'NavBar',
  data() {
    return {
      searchQuery: '',
      mobileMenuActive: false
    };
  },
  computed: {
    ...mapGetters(['cartItemCount'])
  },
  methods: {
    handleSearch() {
      if (this.searchQuery.trim()) {
        this.$router.push(`/?search=${this.searchQuery.trim()}`);
        this.searchQuery = '';
      }
    },
    toggleMobileMenu() {
      this.mobileMenuActive = !this.mobileMenuActive;
    },
    closeMobileMenu() {
      this.mobileMenuActive = false;
    }
  }
};
</script>

<style scoped>
.header {
  background-color: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
  position: sticky;
  top: 0;
  z-index: 100;
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
}

.header-inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 18px 0;
}

.logo a {
  text-decoration: none;
  display: inline-block;
  position: relative;
  overflow: hidden;
}

.logo h1 {
  font-size: 2rem;
  font-weight: 800;
  letter-spacing: -0.5px;
  position: relative;
  transition: var(--transition-normal);
}

.logo span {
  background-image: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  color: transparent;
  font-weight: 900;
  position: relative;
}

.logo span::after {
  content: '';
  position: absolute;
  width: 100%;
  height: 3px;
  bottom: 0;
  left: 0;
  background-image: var(--gradient-primary);
  border-radius: var(--border-radius-full);
  transform: scaleX(0);
  transform-origin: right;
  transition: transform 0.4s cubic-bezier(0.65, 0.05, 0.36, 1);
}

.logo:hover span::after {
  transform: scaleX(1);
  transform-origin: left;
}

.nav-links {
  display: flex;
  list-style: none;
  gap: 5px;
}

.nav-links li {
  position: relative;
}

.nav-links a {
  text-decoration: none;
  color: var(--color-dark);
  font-weight: 600;
  padding: 8px 16px;
  border-radius: var(--border-radius-full);
  transition: var(--transition-normal);
  position: relative;
  display: inline-block;
}

.nav-links a::before {
  content: '';
  position: absolute;
  bottom: 0;
  left: 50%;
  width: 0;
  height: 2px;
  background-image: var(--gradient-primary);
  border-radius: var(--border-radius-full);
  transform: translateX(-50%);
  transition: width 0.3s ease;
  opacity: 0;
}

.nav-links a:hover {
  color: var(--color-primary);
}

.nav-links a:hover::before {
  width: 60%;
  opacity: 1;
}

.nav-links li.dropdown > a {
  display: flex;
  align-items: center;
  gap: 5px;
}

.nav-links li.dropdown > a::after {
  content: '\f107';
  font-family: 'Font Awesome 5 Free';
  font-weight: 900;
  font-size: 0.8rem;
  transition: transform 0.3s ease;
}

.nav-links li.dropdown:hover > a::after {
  transform: rotate(180deg);
}

.dropdown-content {
  display: none;
  position: absolute;
  top: 100%;
  left: 0;
  background-color: white;
  min-width: 220px;
  box-shadow: var(--shadow-md);
  z-index: 10;
  border-radius: var(--border-radius-md);
  padding: 12px 0;
  animation: fadeIn 0.3s ease, slideUp 0.3s ease;
  transform-origin: top center;
  overflow: hidden;
  border: 1px solid rgba(0, 0, 0, 0.05);
}

.dropdown-content a {
  color: var(--color-dark);
  padding: 10px 20px;
  text-decoration: none;
  display: block;
  font-weight: 500;
  transition: all 0.25s ease;
  position: relative;
  overflow: hidden;
  z-index: 1;
}

.dropdown-content a::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  height: 100%;
  width: 4px;
  background-image: var(--gradient-primary);
  transform: scaleY(0);
  transition: transform 0.25s ease;
  z-index: -1;
}

.dropdown-content a:hover {
  background-color: rgba(255, 107, 107, 0.05);
  color: var(--color-primary);
  padding-left: 25px;
}

.dropdown-content a:hover::before {
  transform: scaleY(1);
}

.dropdown:hover .dropdown-content {
  display: block;
}

.header-actions {
  display: flex;
  align-items: center;
  gap: 15px;
}

.search {
  display: flex;
  border: 1px solid rgba(0, 0, 0, 0.08);
  border-radius: var(--border-radius-full);
  overflow: hidden;
  transition: all 0.3s ease;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.03);
}

.search:focus-within {
  box-shadow: 0 0 0 3px rgba(255, 107, 107, 0.2);
  border-color: var(--color-primary-light);
}

.search input {
  padding: 10px 15px;
  border: none;
  outline: none;
  width: 200px;
  font-size: 0.95rem;
  transition: all 0.3s ease;
}

.search button {
  background-image: var(--gradient-primary);
  color: white;
  border: none;
  padding: 0 18px;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.search button:hover {
  filter: brightness(1.1);
}

.cart-icon {
  position: relative;
}

.cart-icon a {
  color: var(--color-dark);
  font-size: 1.3rem;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 42px;
  height: 42px;
  border-radius: 50%;
  background-color: rgba(255, 255, 255, 0.8);
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
  transition: all 0.3s ease;
}

.cart-icon a:hover {
  background-image: var(--gradient-primary);
  color: white;
  transform: translateY(-3px);
  box-shadow: var(--shadow-primary);
}

.cart-count {
  position: absolute;
  top: -5px;
  right: -5px;
  background-image: var(--gradient-primary);
  color: white;
  border-radius: 50%;
  width: 22px;
  height: 22px;
  font-size: 0.75rem;
  display: flex;
  justify-content: center;
  align-items: center;
  box-shadow: 0 2px 5px rgba(255, 107, 107, 0.5);
  font-weight: 600;
  border: 2px solid white;
}

.mobile-toggle {
  display: none;
  font-size: 1.4rem;
  cursor: pointer;
  width: 42px;
  height: 42px;
  border-radius: 50%;
  background-color: rgba(255, 255, 255, 0.8);
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
  display: none;
  align-items: center;
  justify-content: center;
  color: var(--color-dark);
  transition: all 0.3s ease;
}

.mobile-toggle:hover {
  background-image: var(--gradient-primary);
  color: white;
}

.mobile-menu {
  display: none;
  background-color: white;
  padding: 15px 0;
  box-shadow: var(--shadow-md);
  border-top: 1px solid rgba(0, 0, 0, 0.05);
}

.mobile-menu ul {
  list-style: none;
}

.mobile-menu li {
  padding: 0;
  position: relative;
}

.mobile-menu a {
  color: var(--color-dark);
  text-decoration: none;
  display: block;
  padding: 12px 20px;
  border-left: 3px solid transparent;
  transition: all 0.3s ease;
  font-weight: 500;
}

.mobile-menu a:hover {
  background-color: rgba(255, 107, 107, 0.05);
  border-left-color: var(--color-primary);
  color: var(--color-primary);
}

@media (max-width: 992px) {
  .nav {
    display: none;
  }

  .search {
    margin-right: 10px;
  }
  
  .search input {
    width: 160px;
  }

  .mobile-toggle {
    display: flex;
  }

  .mobile-menu {
    display: none;
  }

  .mobile-menu.active {
    display: block;
    animation: fadeIn 0.3s ease;
  }
}

@media (max-width: 768px) {
  .header-actions .search {
    display: none;
  }
  
  .logo h1 {
    font-size: 1.7rem;
  }
  
  .header-inner {
    padding: 15px 0;
  }
}
</style>
