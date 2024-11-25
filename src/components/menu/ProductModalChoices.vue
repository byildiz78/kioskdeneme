<template>
  <div class="choices custom-scrollbar">
    <div class="section-header">
      <div class="caption">Pideler</div>
      <div class="animated-underline"></div>
    </div>
    <div class="choice-products">
      <a @click="$emit('select-choice', $event)">
        <div class="img"
          style="background-image: url('/assets/products/PIDEM_WEB_BANNER_TUM_URUNLER_0020_PATATES_2.jpg');">
        </div>
        <div class="name">PATATESLİ PİDEM</div>
        <div class="price"></div>
      </a>

      <a @click="$emit('select-choice', $event)">
        <div class="img"
          style="background-image: url('/assets/products/PIDEM_WEB_BANNER_TUM_URUNLER_0014_TAVUKLU_2.jpg');">
        </div>
        <div class="name">TAVUKLU PİDEM</div>
        <div class="price"></div>
      </a>
    </div>

    <!-- Other sections... -->
  </div>
</template>

<script setup>
defineEmits(['select-choice'])
</script>

<style scoped>
/* Choice styles from the original Menu.vue */
</style>