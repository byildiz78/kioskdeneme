<template>
  <teleport to="body">
    <div class="modal-overlay" v-if="modelValue">
      <div class="modal-content">
        <div class="product">
          <div class="product-image">
            <img :src="selectedProduct?.src" />
          </div>
          <div class="product-info">
            <div class="product-name">{{ selectedProduct?.name }}</div>
            <div class="product-description">
              <div>
                Onun lezzet durumu: karışık! Acıktığı an ortaya karışık bir şeyler isteyenlere
                kıyma, peynir ve kuşbaşı etiyle Karışık Pidem!
              </div>
            </div>
            <div class="product-price">{{ selectedProduct?.price }} TL</div>
          </div>
        </div>

        <div class="choices">
          <div class="caption">Pideler</div>
          <div class="choice-products">
            <a @click="setChoice">
              <div class="img"
                style="background-image: url('/assets/products/PIDEM_WEB_BANNER_TUM_URUNLER_0020_PATATES_2.jpg');">
              </div>
              <div class="name">PATATESLİ PİDEM</div>
              <div class="price"></div>
            </a>

            <a @click="setChoice">
              <div class="img"
                style="background-image: url('/assets/products/PIDEM_WEB_BANNER_TUM_URUNLER_0014_TAVUKLU_2.jpg');">
              </div>
              <div class="name">TAVUKLU PİDEM</div>
              <div class="price"></div>
            </a>
          </div>

          <div class="caption">İçecekler</div>
          <div class="choice-products">
            <a @click="setChoice">
              <div class="img" style="background-image: url('/assets/products/cappy_visne.jpg');">
              </div>
              <div class="name">KUTU CAPPY VİŞNE 330 ML</div>
              <div class="price"></div>
            </a>

            <a @click="setChoice">
              <div class="img" style="background-image: url('/assets/products/cappy_seftali.jpg');">
              </div>
              <div class="name">KUTU CAPPY ŞEFTALİ 330 ML</div>
              <div class="price"></div>
            </a>

            <a @click="setChoice">
              <div class="img" style="background-image: url('/assets/products/cappy_karisik.jpg');">
              </div>
              <div class="name">KUTU CAPPY KARIŞIK 330 ML</div>
              <div class="price"></div>
            </a>

            <a @click="setChoice">
              <div class="img" style="background-image: url('/assets/products/cocacola.jpg');">
              </div>
              <div class="name">COCA COLA 450 ML</div>
              <div class="price"></div>
            </a>

            <a @click="setChoice">
              <div class="img" style="background-image: url('/assets/products/cocacola.jpg');">
              </div>
              <div class="name">COCA COLA ZERO 450 ML</div>
              <div class="price"></div>
            </a>
          </div>

          <div class="caption">Ekstra</div>
          <div class="choice-products">
            <a @click="setChoice">
              <div class="img" style="background-image: url('/assets/products/TURŞU.jpg');"></div>
              <div class="name">TURŞU</div>
              <div class="price">+10 TL</div>
            </a>

            <a @click="setChoice">
              <div class="img"
                style="background-image: url('/assets/products/PIDEM_WEB_BANNER_TUM_URUNLER_0000_CORBA.jpg');">
              </div>
              <div class="name">ÇORBA</div>
              <div class="price">+49 TL</div>
            </a>
          </div>
        </div>

        <div class="action">
          <div class="buttons cancel">
            <button @click="$emit('update:modelValue', false)">VAZGEÇ</button>
          </div>

          <div class="quantity-options">
            <div class="quantity-button">
              <button @click="setQuantityMinus">
                <Minus :width="65" :height="65" :fill="'#d96000'" :style="'background-color: #fff; border-radius: 100%;'" />
              </button>
            </div>
            <div class="quantity">{{ selectedProduct?.quantity || 1 }}</div>
            <div class="quantity-button">
              <button @click="setQuantityPlus">
                <Plus :width="65" :height="65" :fill="'#d96000'" :style="'background-color: #fff; border-radius: 100%;'" />
              </button>
            </div>
          </div>

          <div class="buttons add-to-basket">
            <button class="step1" @click="handleAddToBasket(true)">ÖDEME YAP</button>
            <button class="step2" @click="handleAddToBasket(false)">SEPETE EKLE</button>
          </div>
        </div>
      </div>
    </div>
  </teleport>
</template>

<script setup>
import Minus from '../icons/Minus.vue'
import Plus from '../icons/Plus.vue'

const props = defineProps({
  modelValue: {
    type: Boolean,
    required: true
  },
  selectedProduct: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['update:modelValue', 'add-to-basket'])

const setQuantityMinus = () => {
  if (props.selectedProduct.quantity > 1) {
    props.selectedProduct.quantity--
  }
}

const setQuantityPlus = () => {
  if (props.selectedProduct.quantity > 0) {
    props.selectedProduct.quantity++
  }
}

const setChoice = (event) => {
  const target = event.target.closest('a')
  if (target) {
    target.classList.toggle('selected')
  }
}

const handleAddToBasket = (withPayment) => {
  emit('add-to-basket', withPayment)
  emit('update:modelValue', false)
}
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  z-index: 2;
  top: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.7);
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: #fff;
  padding: 2rem;
  height: 100vh;
  display: flex;
  flex-direction: column;
  max-width: 1200px;
  width: 90%;
  border-radius: 20px;
  overflow-y: auto;
}

.modal-content .product {
  display: flex;
  gap: 1rem;
  align-items: flex-start;
  flex: 1;
}

.modal-content .product .product-image img {
  width: 275px;
  height: auto;
  border-radius: 1.3rem;
}

.modal-content .product .product-info {
  display: flex;
  flex-direction: column;
}

.modal-content .product .product-name {
  font-size: 2rem;
  font-weight: 700;
  color: #ff8500;
  padding: 0.5rem 1rem;
  background: rgba(255, 133, 0, 0.1);
  border-radius: 10px;
  margin-bottom: 1rem;
}

.modal-content .product .product-description div {
  color: rgb(19, 19, 17);
  border: 1px solid #fd9d34;
  border-radius: 1.3rem;
  padding: 1rem;
  font-size: 1.6rem;
  background-color: #fbf8f4;
  margin: 1rem 0;
  text-align: justify;
}

.modal-content .product .product-price {
  font-size: 2rem;
  font-weight: 700;
  color: #ff8500;
  text-align: right;
}

.modal-content .action {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  background-color: rgb(233, 152, 3);
  border-radius: 1.3rem;
  margin-top: 1rem;
  flex: 1;
}

.modal-content .action .quantity-options {
  display: flex;
  margin: 0.6rem 1rem 0.3rem 1rem;
}

.modal-content .action .quantity-options .quantity-button button {
  border: none;
  padding: 0;
  background-color: transparent;
  cursor: pointer;
  transition: transform 0.2s;
}

.modal-content .action .quantity-options .quantity-button button:active {
  transform: scale(0.95);
}

.modal-content .action .quantity-options .quantity {
  font-size: 2.3rem;
  color: #fff;
  font-weight: 700;
  padding: 0 1rem;
}

.modal-content .action .buttons {
  display: flex;
}

.modal-content .action .buttons button {
  border: none;
  background-color: #d96000;
  color: #fff;
  font-weight: 700;
  font-size: 1.5rem;
  padding: 1rem;
  width: 200px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.modal-content .action .buttons.cancel button {
  border-radius: 1.3rem 0 0 1.3rem;
}

.modal-content .action .buttons.add-to-basket .step1 {
  border-radius: 0;
  background-color: #49b35a;
}

.modal-content .action .buttons.add-to-basket .step2 {
  border-radius: 0 1.3rem 1.3rem 0;
}

.modal-content .action .buttons button:hover {
  filter: brightness(1.1);
}

.modal-content .action .buttons button:active {
  transform: scale(0.98);
}

.modal-content .choices {
  display: flex;
  flex-direction: column;
  flex: 17;
  overflow-y: auto;
  padding-right: 1rem;
}

.modal-content .choices .caption {
  color: #d96000;
  font-size: 2rem;
  font-weight: 700;
  margin: 0.5rem 0 1rem 0;
  border-bottom: 4px solid #ff8500;
  position: relative;
}

.modal-content .choices .caption::after {
  content: '';
  position: absolute;
  bottom: -4px;
  left: 0;
  width: 50%;
  height: 4px;
  background: linear-gradient(90deg, #ff8500, transparent);
  animation: shine 2s infinite;
}

@keyframes shine {
  0% {
    left: -100%;
  }
  100% {
    left: 100%;
  }
}

.modal-content a {
  display: flex;
  flex-direction: column;
  background-color: #f3f1f1;
  border-radius: 1.3rem;
  padding: 1rem;
  border: 2px solid #f3f1f1;
  align-items: center;
  transition: all 0.3s ease;
}

.modal-content a.selected {
  background-color: #ebeaea;
  border-color: #fd7f18;
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(253, 127, 24, 0.2);
}

.modal-content .choices .choice-products {
  display: grid;
  gap: 1rem;
  margin: 0 0 1rem;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
}

.modal-content .choices .choice-products .img {
  width: 130px;
  height: 130px;
  border-radius: 1.3rem;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  transition: transform 0.3s ease;
}

.modal-content .choices .choice-products a:hover .img {
  transform: scale(1.05);
}

.modal-content .choices .choice-products .name {
  font-size: 1.1rem;
  font-weight: 500;
  text-align: center;
  margin-top: 0.5rem;
}

.modal-content .choices .choice-products .price {
  font-size: 1.1rem;
  font-weight: 500;
  text-align: center;
  color: #d96000;
}
</style>