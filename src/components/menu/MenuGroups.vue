<template>
  <div class="menu-groups">
    <a v-for="group in groups" 
       :key="group.id" 
       :class="{ active: (group.id == modelValue?.id) }"
       @click="$emit('update:modelValue', group)">
      <img :src="group.src" />
    </a>
  </div>
</template>

<script setup>
const props = defineProps({
  modelValue: {
    type: Object,
    required: true
  },
  groups: {
    type: Array,
    required: true
  }
})

defineEmits(['update:modelValue'])
</script>

<style scoped>
.menu-groups {
  flex: 2;
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
  padding: 1rem;
  justify-content: center;
}

.menu-groups img {
  width: 100%;
  border-radius: 1.3rem;
  transition: all 0.3s ease;
}

.menu-groups a.active img,
.menu-groups a:active img {
  box-shadow: 0px 0px 9px 3px #d96000;
  transform: scale(1.02);
}
</style>