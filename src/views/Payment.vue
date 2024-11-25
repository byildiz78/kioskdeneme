<template>
  <div class="payment">
    <div class="logo">
      <Logo :width="200" />
    </div>

    <div class="payment-content">
      <div class="payment-header animate__animated animate__fadeInDown">
        <div class="slogan">
          <span>Siparişinizi vermeye çok az kaldı!</span>
          <div class="animated-line"></div>
        </div>

        <div class="question">
          <span>Hangi ödeme şekliyle ödemek istersiniz?</span>
        </div>

        <div class="price-cards">
          <div class="price-card animate__animated animate__fadeInLeft">
            <div class="currency-icon">₺</div>
            <div class="amount">{{ basket.total.toFixed(2) }}</div>
            <div class="currency">TL</div>
          </div>

          <div class="price-card animate__animated animate__fadeInUp">
            <div class="currency-icon">$</div>
            <div class="amount">{{ (basket.total / 31.5).toFixed(2) }}</div>
            <div class="currency">USD</div>
          </div>

          <div class="price-card animate__animated animate__fadeInRight">
            <div class="currency-icon">€</div>
            <div class="amount">{{ (basket.total / 34.2).toFixed(2) }}</div>
            <div class="currency">EUR</div>
          </div>
        </div>
      </div>

      <div class="payment-options">
        <button class="payment-button credit-card animate__animated animate__fadeInLeft" 
                @click="goPaymentWaiting">
          <div class="button-content">
            <CreditCard :width="170" :fill="'#fff'" />
            <span>KREDİ KARTI</span>
          </div>
          <div class="hover-effect"></div>
        </button>

        <button class="payment-button meal-card animate__animated animate__fadeInRight" 
                @click="toggleMealOptions">
          <div class="button-content">
            <MealCard :width="170" :fill="'#fff'" />
            <span>YEMEK ÇEKİ</span>
          </div>
          <div class="hover-effect"></div>
        </button>
      </div>

      <Transition name="fade">
        <div v-if="showMealPaymentTypes" class="meal-options animate__animated animate__fadeIn">
          <div class="meal-options-grid">
            <button v-for="(option, index) in mealOptions" 
                    :key="index"
                    class="meal-option"
                    @click="goPaymentWaiting">
              <component :is="option.component" :width="250" :height="100" />
              <div class="shine"></div>
            </button>
          </div>
        </div>
      </Transition>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { useApplicationStore } from '@/stores/application'
import { storeToRefs } from 'pinia'

import Logo from '../components/icons/Logo.vue'
import CreditCard from '../components/icons/CreditCard.vue'
import MealCard from '../components/icons/MealCard.vue'
import Metropolcard from '../components/icons/brands/Metropolcard.vue'
import Multinet from '../components/icons/brands/Multinet.vue'
import Pluxee from '../components/icons/brands/Pluxee.vue'
import Setcard from '../components/icons/brands/Setcard.vue'

const router = useRouter()
const applicationStore = useApplicationStore()
const { basket } = storeToRefs(applicationStore)

const showMealPaymentTypes = ref(false)

const mealOptions = [
  { component: Metropolcard },
  { component: Multinet },
  { component: Pluxee },
  { component: Setcard }
]

const goPaymentWaiting = () => {
  router.push({ name: 'payment-waiting' })
}

const toggleMealOptions = () => {
  showMealPaymentTypes.value = !showMealPaymentTypes.value
}
</script>

<style scoped>
@import 'animate.css';

.payment {
  width: 100vw;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  background: linear-gradient(135deg, #f5f7fa 0%, #e4e7eb 100%);
}

.logo {
  background-color: #000;
  padding: 1rem 0;
  display: flex;
  justify-content: center;
}

.payment-content {
  flex: 1;
  padding: 2rem;
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.payment-header {
  text-align: center;
}

.slogan {
  margin-bottom: 1rem;
}

.slogan span {
  font-size: 1.8rem;
  font-weight: 600;
  color: #2c3e50;
  background: linear-gradient(45deg, #3498db, #2980b9);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.question {
  margin: 2rem 0;
}

.question span {
  font-size: 2.5rem;
  font-weight: 700;
  color: #2c3e50;
  background: linear-gradient(45deg, #ff8f00, #ff6f00);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.animated-line {
  height: 3px;
  background: linear-gradient(90deg, transparent, #3498db, transparent);
  margin: 0.5rem auto;
  width: 50%;
  position: relative;
  overflow: hidden;
}

.animated-line::after {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.8), transparent);
  animation: shine 2s infinite;
}

@keyframes shine {
  to { left: 100%; }
}

.price-cards {
  display: flex;
  justify-content: center;
  gap: 2rem;
  margin: 2rem 0;
}

.price-card {
  background: white;
  padding: 1.5rem 2rem;
  border-radius: 15px;
  box-shadow: 0 10px 20px rgba(0,0,0,0.1);
  display: flex;
  align-items: center;
  gap: 0.5rem;
  min-width: 200px;
  position: relative;
  overflow: hidden;
}

.price-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 4px;
  background: linear-gradient(90deg, #ff8f00, #ff6f00);
}

.currency-icon {
  font-size: 2rem;
  font-weight: 700;
  color: #ff8f00;
}

.amount {
  font-size: 2.5rem;
  font-weight: 800;
  color: #2c3e50;
}

.currency {
  font-size: 1.2rem;
  font-weight: 600;
  color: #7f8c8d;
}

.payment-options {
  display: flex;
  justify-content: center;
  gap: 2rem;
  margin: 2rem 0;
}

.payment-button {
  position: relative;
  width: 300px;
  padding: 2rem;
  border: none;
  border-radius: 20px;
  cursor: pointer;
  overflow: hidden;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.payment-button.credit-card {
  background: linear-gradient(135deg, #43a047, #2e7d32);
}

.payment-button.meal-card {
  background: linear-gradient(135deg, #1e88e5, #1565c0);
}

.button-content {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
}

.payment-button span {
  color: white;
  font-size: 1.5rem;
  font-weight: 600;
  text-shadow: 1px 1px 2px rgba(0,0,0,0.2);
}

.hover-effect {
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
  transition: 0.5s;
}

.payment-button:hover {
  transform: translateY(-5px);
  box-shadow: 0 15px 30px rgba(0,0,0,0.2);
}

.payment-button:hover .hover-effect {
  left: 100%;
}

.meal-options {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  background: rgba(255,255,255,0.95);
  backdrop-filter: blur(10px);
  padding: 2rem;
  border-radius: 20px 20px 0 0;
  box-shadow: 0 -10px 20px rgba(0,0,0,0.1);
}

.meal-options-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1rem;
  max-width: 1200px;
  margin: 0 auto;
}

.meal-option {
  background: white;
  border: none;
  border-radius: 15px;
  padding: 1.5rem;
  cursor: pointer;
  position: relative;
  overflow: hidden;
  transition: transform 0.3s ease;
  box-shadow: 0 5px 15px rgba(0,0,0,0.1);
}

.meal-option:hover {
  transform: translateY(-3px);
}

.shine {
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.5), transparent);
  animation: shine-effect 2s infinite;
}

@keyframes shine-effect {
  to { left: 100%; }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>