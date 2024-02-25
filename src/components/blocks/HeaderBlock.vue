<template>
  <header :class="{
    'container': true,
    'header': true,
    'basket': basketList,
    'description': descriptionItem
  }">
    <svg :class="{
        'header__button': true,
        'basket__button': basketList,
        'description__button': descriptionItem
      }"
      style="cursor: pointer;"
      @click="router.back()"
      width="32" height="32" viewBox="0 0 32 32" fill="none" xmlns="http://www.w3.org/2000/svg">
      <circle cx="16" cy="16" r="15.5" transform="rotate(180 16 16)" stroke="#D58C51"/>
      <g clip-path="url(#clip0_0_127)">
      <path d="M14.6514 13.0469C14.7868 12.9115 14.9998 12.9115 15.1352 13.0469C15.2659 13.1776 15.2659 13.3953 15.1352 13.5258L12.6823 15.9786H20.7765C20.9652 15.9786 21.12 16.1286 21.12 16.3173C21.12 16.506 20.9652 16.6608 20.7765 16.6608H12.6823L15.1352 19.1091C15.2659 19.2445 15.2659 19.4624 15.1352 19.5929C14.9998 19.7282 14.7868 19.7282 14.6514 19.5929L11.6179 16.5593C11.4871 16.4286 11.4871 16.2109 11.6179 16.0804L14.6514 13.0469Z" fill="#D58C51"/>
      </g>
      <defs>
      <clipPath id="clip0_0_127">
      <rect width="9.6" height="9.6" fill="white" transform="translate(11.52 11.52)"/>
      </clipPath>
      </defs>
    </svg>

    <h1 class="header__title">{{ title }}</h1>
    <div :class="{
      'header__action': true,
    }">
      <div :class="{
        'header__action': true,
        'basket__action': basketList
      }">
        <div class="header__info">
          <p class="header__text">{{ productsCount }} товара</p>
          <p class="header__text">на сумму {{ productsAmount.toLocaleString() }} ₽</p>
        </div>
        <router-link to="/basket">
          <basketIcon />
        </router-link>
      </div>
      <ButtionUI
        text="Выйти"
        rectangleActive
        @click="deautorization"
      />
    </div>
  </header>
</template>

<script>
import basketIcon from '@/components/icons/basketIcon.vue'
import ButtionUI from '@/components/ui/ButtonUI.vue'
import { computed } from 'vue'
import { useStore } from 'vuex'
import { useRouter } from 'vue-router'

export default {
  name: 'HeaderBlock',
  components: {
    basketIcon,
    ButtionUI
  },
  props: {
    basketList: {
      type: Boolean,
      default: false
    },
    descriptionItem: {
      type: Boolean,
      default: false
    },
    title: {
      type: String,
      default: ''
    }
  },
  setup () {
    const store = useStore()
    const router = useRouter()

    const productsAmount = computed(() => {
      return store.getters.getAllPricePoductsInBasket
    })
    const productsCount = computed(() => {
      return store.getters.getCountProductsInBasket
    })
    const deautorization = () => {
      store.commit('SetDeautorization')
      store.commit('SetClearBasketProducts')
      router.push('/auth')
    }
    return {
      router,
      productsAmount,
      productsCount,
      deautorization
    }
  }
}
</script>

<style lang="scss" scoped>
  .header {
    padding: 54px 70px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: var(--color-main);
    background: var(--color-main-background);
    &__title {
      font-size: 31px;
      font-weight: 700;
    }
    &__action {
      display: flex;
      align-items: center;
      gap: 20px;
    }
    &__info {
      display: flex;
      flex-direction: column;
      align-items: flex-end;
    }
    &__text {
      font-size: 17px;
      font-weight: 500;
    }
    &__button {
      display: none;
    }
  }
  .basket {
    justify-content: center;
    gap: 63px;
    &__action {
      display: none;
    }
    &__button {
      display: inline-flex;
    }
  }

  .description {
    background: transparent;
    position: absolute;
    width: calc(100vw - 140px);
    &__button {
      display: inline-flex;
    }
  }
</style>
