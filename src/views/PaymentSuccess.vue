<template>
  <div class="payment-success">
    <div class="logo">
      <Logo :width="200" />
    </div>

    <div class="success-content">
      <div class="animation-container animate__animated animate__fadeIn">
        <DotLottieVue 
          class="success-animation"
          autoplay 
          :data="PaymentSuccess" />
      </div>

      <div class="order-details animate__animated animate__fadeInUp">
        <div class="success-message">
          <h1>Siparişiniz Alındı!</h1>
          <p>Siparişiniz başarıyla hazırlanmaya başlandı.</p>
        </div>

        <div class="order-number">
          <div class="label">SİPARİŞ NUMARASI</div>
          <div class="number">
            <span class="digit" v-for="(digit, index) in '5754'" :key="index"
                  :style="{ animationDelay: `${index * 0.1}s` }">
              {{ digit }}
            </span>
          </div>
        </div>

        <div class="estimated-time">
          <div class="time-icon">
            <Clock :width="32" :height="32" :fill="'#ff8500'" />
          </div>
          <div class="time-details">
            <span class="label">Tahmini Hazırlanma Süresi</span>
            <span class="time">15-20 dakika</span>
          </div>
        </div>

        <div class="order-steps">
          <div class="step completed">
            <div class="step-icon">✓</div>
            <div class="step-label">Sipariş Alındı</div>
          </div>
          <div class="step-connector active"></div>
          <div class="step active">
            <div class="step-icon">
              <Chef :width="24" :height="24" :fill="'#fff'" />
            </div>
            <div class="step-label">Hazırlanıyor</div>
          </div>
          <div class="step-connector"></div>
          <div class="step">
            <div class="step-icon">
              <Bag :width="24" :height="24" :fill="'#636e72'" />
            </div>
            <div class="step-label">Hazır</div>
          </div>
        </div>
      </div>

      <button @click="goIntro" class="home-button animate__animated animate__fadeInUp">
        <div class="button-content">
          <Home :width="24" :height="24" :fill="'#ff8500'" />
          <span>ANA EKRANA DÖN</span>
        </div>
        <div class="button-shine"></div>
      </button>
    </div>
  </div>
</template>

<script setup>
import PaymentSuccess from '../assets/img/payment-success.json'
import Logo from '../components/icons/Logo.vue'
import Clock from '../components/icons/Clock.vue'
import Chef from '../components/icons/Chef.vue'
import Bag from '../components/icons/Bag.vue'
import Home from '../components/icons/Home.vue'
import { DotLottieVue } from '@lottiefiles/dotlottie-vue'
import { onMounted } from 'vue'
import { useApplicationStore } from '@/stores/application'
import { useRouter } from 'vue-router'
import 'animate.css'

const router = useRouter()
const applicationStore = useApplicationStore()
const { orderCancelConfirmOk } = applicationStore

onMounted(() => {
  setTimeout(goIntro, 15000)
})

const goIntro = () => {
  orderCancelConfirmOk()
  router.push({ name: 'intro' })
}
</script>

<style scoped>
.payment-success {
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

.success-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  gap: 2rem;
}

.animation-container {
  width: 300px;
  height: 300px;
  margin-bottom: 2rem;
}

.success-animation {
  width: 100%;
  height: 100%;
}

.order-details {
  background: white;
  padding: 2rem;
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 600px;
}

.success-message {
  text-align: center;
  margin-bottom: 2rem;
}

.success-message h1 {
  font-size: 2.5rem;
  color: #2d3436;
  margin-bottom: 0.5rem;
  background: linear-gradient(45deg, #ff8500, #ff6f00);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.success-message p {
  color: #636e72;
  font-size: 1.2rem;
}

.order-number {
  text-align: center;
  margin: 2rem 0;
  padding: 2rem;
  background: linear-gradient(135deg, rgba(255, 133, 0, 0.1), rgba(255, 111, 0, 0.1));
  border-radius: 15px;
}

.order-number .label {
  font-size: 1.2rem;
  color: #636e72;
  margin-bottom: 1rem;
}

.order-number .number {
  display: flex;
  justify-content: center;
  gap: 1rem;
}

.order-number .digit {
  font-size: 3.5rem;
  font-weight: 800;
  color: #ff8500;
  animation: fadeInUp 0.5s ease forwards;
  opacity: 0;
  transform: translateY(20px);
}

@keyframes fadeInUp {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.estimated-time {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 1rem;
  background: #f8f9fa;
  border-radius: 10px;
  margin: 2rem 0;
}

.time-details {
  display: flex;
  flex-direction: column;
}

.time-details .label {
  font-size: 0.9rem;
  color: #636e72;
}

.time-details .time {
  font-size: 1.2rem;
  font-weight: 700;
  color: #2d3436;
}

.order-steps {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 2rem 0;
}

.step {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  flex: 1;
}

.step-icon {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: #636e72;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  font-weight: 700;
}

.step.completed .step-icon {
  background: #49b35a;
}

.step.active .step-icon {
  background: #ff8500;
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0% {
    box-shadow: 0 0 0 0 rgba(255, 133, 0, 0.4);
  }
  70% {
    box-shadow: 0 0 0 10px rgba(255, 133, 0, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(255, 133, 0, 0);
  }
}

.step-connector {
  flex: 1;
  height: 3px;
  background: #636e72;
  margin: 0 10px;
  position: relative;
  top: -20px;
}

.step-connector.active {
  background: #ff8500;
}

.step-label {
  font-size: 0.9rem;
  color: #636e72;
  text-align: center;
}

.home-button {
  position: relative;
  padding: 1rem 2rem;
  border: 2px solid #ff8500;
  background: white;
  border-radius: 15px;
  cursor: pointer;
  overflow: hidden;
  transition: all 0.3s ease;
}

.button-content {
  position: relative;
  z-index: 1;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.button-content span {
  color: #ff8500;
  font-weight: 700;
}

.button-shine {
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: radial-gradient(circle, rgba(255, 133, 0, 0.2) 0%, transparent 70%);
  transform: rotate(45deg);
  transition: 0.3s;
}

.home-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(255, 133, 0, 0.2);
}

.home-button:hover .button-shine {
  transform: rotate(45deg) translate(50%, 50%);
}
</style>