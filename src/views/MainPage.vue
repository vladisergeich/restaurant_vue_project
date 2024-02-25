<template>
  <HeaderBlock title="НАША ПРОДУКЦИЯ" />
  <ItemList />
</template>

<script>
import HeaderBlock from '@/components/blocks/HeaderBlock.vue'
import ItemList from '@/components/blocks/ItemList.vue'
import { computed, onBeforeMount } from 'vue'
import { useStore } from 'vuex'
import { useRouter } from 'vue-router'

export default {
  name: 'MainPage',
  components: {
    HeaderBlock,
    ItemList
  },
  props: {
  },
  setup () {
    const store = useStore()
    const router = useRouter()

    const auth = computed(() => {
      return store.getters.getAuth
    })
    onBeforeMount(() => {
      if (auth.value === 'false') {
        router.push('/auth')
      }
    })
  }
}
</script>
