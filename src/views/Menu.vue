<template>
  <div>
    <div class="menu-view">
      <MenuHeader ref="header" />

      <div class="menu">
        <MenuGroups 
          v-model="selectedGroup"
          :groups="groups" />

        <MenuItems 
          ref="items"
          :selected-group="selectedGroup"
          :menu-items="menuItems"
          @select-product="setSelectedProduct" />
      </div>
    </div>

    <ProductModal
      v-model="modalIsOpen"
      :selected-product="selectedProduct"
      @add-to-basket="handleAddToBasket" />

    <BottomNav :showBasket="true" />
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'
import { useRouter } from 'vue-router'
import { useApplicationStore } from '@/stores/application'
import { storeToRefs } from 'pinia'

import BottomNav from '../components/BottomNav.vue'
import MenuHeader from '../components/menu/MenuHeader.vue'
import MenuGroups from '../components/menu/MenuGroups.vue'
import MenuItems from '../components/menu/MenuItems.vue'
import ProductModal from '../components/menu/ProductModal.vue'

const router = useRouter()
const applicationStore = useApplicationStore()
const { impairedActive } = storeToRefs(applicationStore)
const { addToBasket } = applicationStore

const header = ref(null)
const items = ref(null)

const selectedGroup = ref({ id: 0, name: "" })
const selectedProduct = ref(null)
const modalIsOpen = ref(false)

const groups = ref([
  { id: 2, name: "pideler", src: "/assets/groups/mp-pideler-banner.jpg" },
  { id: 1, name: "tüm ürünler", src: "/assets/groups/mp-tum-urunler-banner.jpg" },
  { id: 3, name: "fırsatlar", src: "/assets/groups/mp-firsatlar-banner.jpg" }
])

const menuItems = ref([
  {
    id: 1,
    groupIds: [1, 2],
    name: "PATATESLİ PİDEM",
    price: 77,
    src: "/assets/products/PIDEM_WEB_BANNER_TUM_URUNLER_0020_PATATES_2.jpg"
  },
  {
    id: 2,
    groupIds: [1, 2],
    name: "LAHMACUN",
    price: 79,
    src: "/assets/products/lahmacun.jpg"
  },
  {
    id: 3,
    groupIds: [1, 2],
    name: "TAVUKLU PATATESLİM",
    price: 119,
    src: "/assets/products/PIDEM_WEB_BANNER_TUM_URUNLER_0018_TAVUKLU_PATATESLIM.jpg"
  },
  {
    id: 4,
    groupIds: [1, 2],
    name: "KIYMALI PATATESLİM",
    price: 125,
    src: "/assets/products/PIDEM_WEB_BANNER_TUM_URUNLER_0016_KIYMA_PATATES_2.jpg"
  },
  {
    id: 5,
    groupIds: [1, 2],
    name: "TAVUKLU PİDEM",
    price: 135,
    src: "/assets/products/PIDEM_WEB_BANNER_TUM_URUNLER_0014_TAVUKLU_2.jpg"
  },
  {
    id: 6,
    groupIds: [1, 2],
    name: "PEYNİRLİ PİDEM",
    price: 159,
    src: "/assets/products/PIDEM_WEB_BANNER_TUM_URUNLER_0012_PEYNIRLI_2.jpg"
  },
  {
    id: 7,
    groupIds: [1, 2],
    name: "KIYMALI PİDEM",
    price: 159,
    src: "/assets/products/PIDEM_WEB_BANNER_TUM_URUNLER_0010_KIYMALI_2.jpg"
  },
  {
    id: 8,
    groupIds: [1, 2],
    name: "SUCUKLU ŞAHANEM",
    price: 165,
    src: "/assets/products/PIDEM_WEB_BANNER_TUM_URUNLER_0008_SUCUKLU_SAHANE_2.jpg"
  },
  {
    id: 9,
    groupIds: [1, 2],
    name: "KARIŞIK PİDEM",
    price: 169,
    src: "/assets/products/PIDEM_WEB_BANNER_TUM_URUNLER_0006_KARISIK_2.jpg"
  },
  {
    id: 10,
    groupIds: [1, 2],
    name: "KUŞBAŞILI PİDEM",
    price: 179,
    src: "/assets/products/PIDEM_WEB_BANNER_TUM_URUNLER_0004_KUSBASI_2.jpg"
  },
  {
    id: 11,
    groupIds: [1, 2],
    name: "KUŞBAŞILI PEYNİRLİM",
    price: 189,
    src: "/assets/products/PIDEM_WEB_BANNER_TUM_URUNLER_0002_KUSBASI_PEYNIR_2.jpg"
  },
  {
    id: 12,
    groupIds: [1, 3],
    name: "PATATESLİ MENÜM",
    price: 87,
    src: "/assets/products/PIDEM_WEB_BANNER_TUM_URUNLER_0021_PATATES_1.jpg"
  },
  {
    id: 13,
    groupIds: [1, 3],
    name: "LAHMACUN MENÜM",
    price: 99,
    src: "/assets/products/lahmacun_menu_24234.jpg"
  },
  {
    id: 14,
    groupIds: [1, 3],
    name: "TAVUKLU PATATESLİ MENÜM",
    price: 129,
    src: "/assets/products/TAVUKLU-PATATESLI-MENUM.jpg"
  },
  {
    id: 15,
    groupIds: [1, 3],
    name: "KIYMALI PATATESLİ MENÜM",
    price: 135,
    src: "/assets/products/PIDEM_WEB_BANNER_TUM_URUNLER_0017_KIYMA_PATATES_1.jpg"
  },
  {
    id: 16,
    groupIds: [1, 3],
    name: "TAVUKLU MENÜM",
    price: 145,
    src: "/assets/products/PIDEM_WEB_BANNER_TUM_URUNLER_0015_TAVUKLU_1.jpg"
  },
  {
    id: 17,
    groupIds: [1, 3],
    name: "KARIŞIK 2LİM",
    price: 220,
    src: "/assets/products/PIDEM_WEB_BANNER_TUM_URUNLER_0019_KARISIK_IKILIM.jpg"
  },
  {
    id: 18,
    groupIds: [1, 3],
    name: "ETSEVER 2LİM",
    price: 290,
    src: "/assets/products/yeni_WEB_BANNER_TUM_URUNLER_0001_ETSEVER_IKILIM.jpg"
  },
  {
    id: 19,
    groupIds: [1],
    name: "İLAVE LAHMACUN GARNİTÜRÜ",
    price: 7,
    src: "/assets/products/garnitur.jpg"
  },
  {
    id: 20,
    groupIds: [1],
    name: "TURŞU",
    price: 10,
    src: "/assets/products/TURŞU.jpg"
  },
  {
    id: 21,
    groupIds: [1],
    name: "ÇORBA",
    price: 49,
    src: "/assets/products/PIDEM_WEB_BANNER_TUM_URUNLER_0000_CORBA.jpg"
  },
  {
    id: 22,
    groupIds: [1],
    name: "SÜTLAÇ",
    price: 59,
    src: "/assets/products/Trendyol-web-sutlac-2024-1000x1000px.jpg"
  },
  {
    id: 23,
    groupIds: [1],
    name: "KÜNEFE",
    price: 69,
    src: "/assets/products/kunefe.jpg"
  },
  {
    id: 24,
    groupIds: [1],
    name: "ÇAY",
    price: 10,
    src: "/assets/products/PIDEM_WEB_BANNER_TUM_URUNLER_BARDAK.jpg"
  },
  {
    id: 25,
    groupIds: [1],
    name: "SU",
    price: 15,
    src: "/assets/products/yenisu.jpg"
  },
  {
    id: 26,
    groupIds: [1],
    name: "SODA",
    price: 25,
    src: "/assets/products/soda.jpg"
  },
  {
    id: 27,
    groupIds: [1],
    name: "FANTA 450 ML",
    price: 39,
    src: "/assets/products/fanta-fuse.jpg"
  },
  {
    id: 28,
    groupIds: [1],
    name: "FUSE TEA ŞEFTALİ 450 ML",
    price: 35,
    src: "/assets/products/cocacola.jpg"
  },
  {
    id: 29,
    groupIds: [1],
    name: "AYRAN 285 ML",
    price: 35,
    src: "/assets/products/ayran-pidem.jpg"
  },
  {
    id: 30,
    groupIds: [1],
    name: "COCA COLA 450 ML",
    price: 39,
    src: "/assets/products/cocacola.jpg"
  },
  {
    id: 31,
    groupIds: [1],
    name: "COCA COLA ZERO 450 ML",
    price: 39,
    src: "/assets/products/cocacola.jpg"
  },
  {
    id: 32,
    groupIds: [1],
    name: "KUTU CAPPY KARIŞIK 330 ML",
    price: 39,
    src: "/assets/products/cappy_karisik.jpg"
  },
  {
    id: 33,
    groupIds: [1],
    name: "KUTU CAPPY ŞEFTALİ 330 ML",
    price: 39,
    src: "/assets/products/cappy_seftali.jpg"
  },
  {
    id: 34,
    groupIds: [1],
    name: "KUTU CAPPY VİŞNE 330 ML",
    price: 39,
    src: "/assets/products/cappy_visne.jpg"
  }
])

onMounted(() => {
  const firstGroup = groups.value[0]
  selectedGroup.value = firstGroup
  calculateHeights()
})

watch(impairedActive, () => {
  setTimeout(calculateHeights, 500)
})

const setSelectedProduct = (product) => {
  selectedProduct.value = { ...product, quantity: 1 }
  modalIsOpen.value = true
}

const handleAddToBasket = (withPayment) => {
  addToBasket(selectedProduct.value)
  if (withPayment) {
    router.push({ name: 'payment' })
  }
}

const calculateHeights = () => {
  if (header.value?.logo && header.value?.carousel && items.value?.menuCaption) {
    const app = document.getElementById('app')
    const appStyle = getComputedStyle(app)
    const appMarginTop = parseFloat(appStyle.marginTop)

    const bottomNav = document.getElementById('bottom-nav')
    const bottomNavStyle = getComputedStyle(bottomNav)
    const bottomNavHeight = parseFloat(bottomNavStyle.height)

    const logoHeight = header.value.logo.clientHeight
    const carouselHeight = header.value.carousel.clientHeight
    const menuCaptionHeight = items.value.menuCaption.clientHeight

    const totalHeight = logoHeight + carouselHeight + menuCaptionHeight + bottomNavHeight + appMarginTop + 20

    if (items.value?.menuContent) {
      items.value.menuContent.style.height = `calc(100vh - ${totalHeight}px)`
    }
  }
}
</script>

<style scoped>
.menu-view {
  width: 100vw;
  display: flex;
  flex-direction: column;
}

.menu {
  flex: 8;
  display: flex;
  flex-direction: row;
  padding-bottom: 5rem;
}
</style>