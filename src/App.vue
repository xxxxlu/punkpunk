<template>
  <div id="app">
    <NavBar />
    <router-view/>
    <Footer />
  </div>
</template>

<script>
import NavBar from './components/NavBar.vue';
import Footer from './components/Footer.vue';

export default {
  name: 'App',
  components: {
    NavBar,
    Footer
  }
}
</script>

<style>
:root {
  --gradient-primary: linear-gradient(135deg, #ff6b6b, #ff8e8e);
  --gradient-secondary: linear-gradient(135deg, #7049f0, #9d6ffa);
  --gradient-accent: linear-gradient(135deg, #0cc3e7, #5edcff);
  --gradient-dark: linear-gradient(135deg, #3c3f45, #525a66);
  --color-primary: #ff6b6b;
  --color-primary-light: #ffe0e0;
  --color-secondary: #7049f0;
  --color-accent: #0cc3e7;
  --color-dark: #3c3f45;
  --color-light: #f8f9fa;
  --color-gray: #a0a0a0;
  --shadow-sm: 0 2px 8px rgba(0, 0, 0, 0.08);
  --shadow-md: 0 4px 12px rgba(0, 0, 0, 0.12);
  --shadow-lg: 0 8px 24px rgba(0, 0, 0, 0.15);
  --shadow-primary: 0 5px 15px rgba(255, 107, 107, 0.3);
  --shadow-secondary: 0 5px 15px rgba(112, 73, 240, 0.3);
  --border-radius-sm: 6px;
  --border-radius-md: 12px;
  --border-radius-lg: 20px;
  --border-radius-full: 9999px;
  --transition-fast: 0.2s ease;
  --transition-normal: 0.3s ease;
  --transition-slow: 0.5s ease;
  --font-primary: 'Roboto', -apple-system, BlinkMacSystemFont, sans-serif;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: var(--font-primary);
  background-color: var(--color-light);
  color: var(--color-dark);
  background-image: radial-gradient(circle at 50% 50%, #ffffff, #f8f9fa, #f2f3f5);
  background-attachment: fixed;
}

#app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.btn {
  display: inline-block;
  padding: 10px 20px;
  border: none;
  border-radius: var(--border-radius-md);
  cursor: pointer;
  font-weight: 600;
  text-align: center;
  transition: all var(--transition-normal);
  position: relative;
  overflow: hidden;
  z-index: 1;
}

.btn::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(255, 255, 255, 0.1);
  transform: translateX(-100%);
  transition: transform 0.5s ease;
  z-index: -1;
}

.btn:hover::before {
  transform: translateX(0);
}

.btn-primary {
  background-image: var(--gradient-primary);
  color: white;
  box-shadow: var(--shadow-primary);
}

.btn-primary:hover {
  transform: translateY(-3px);
  box-shadow: 0 7px 20px rgba(255, 107, 107, 0.4);
}

.btn-secondary {
  background-image: var(--gradient-secondary);
  color: white;
  box-shadow: var(--shadow-secondary);
}

.btn-secondary:hover {
  transform: translateY(-3px);
  box-shadow: 0 7px 20px rgba(112, 73, 240, 0.4);
}

.btn-outline {
  background: transparent;
  border: 2px solid var(--color-primary);
  color: var(--color-primary);
}

.btn-outline:hover {
  background-color: var(--color-primary-light);
}

.btn-block {
  display: block;
  width: 100%;
}

.text-center {
  text-align: center;
}

.text-gradient {
  background-image: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  color: transparent;
}

.mb-2 {
  margin-bottom: 0.5rem;
}

.mb-3 {
  margin-bottom: 1rem;
}

.mb-4 {
  margin-bottom: 1.5rem;
}

.mt-2 {
  margin-top: 0.5rem;
}

.mt-3 {
  margin-top: 1rem;
}

.mt-4 {
  margin-top: 1.5rem;
}

.section-title {
  font-size: 2rem;
  margin-bottom: 2rem;
  position: relative;
  padding-bottom: 12px;
  text-align: center;
  background-image: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.section-title::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 80px;
  height: 3px;
  background-image: var(--gradient-primary);
  border-radius: var(--border-radius-full);
}

/* 添加动画效果 */
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideUp {
  from { transform: translateY(20px); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}

@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.05); }
  100% { transform: scale(1); }
}

.card {
  background-color: white;
  border-radius: var(--border-radius-md);
  overflow: hidden;
  box-shadow: var(--shadow-sm);
  transition: transform var(--transition-normal), box-shadow var(--transition-normal);
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: var(--shadow-md);
}

/* 自定义滚动条 */
::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}

::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: var(--border-radius-full);
}

::-webkit-scrollbar-thumb {
  background-image: var(--gradient-primary);
  border-radius: var(--border-radius-full);
}

::-webkit-scrollbar-thumb:hover {
  background-image: var(--gradient-secondary);
}
</style>
