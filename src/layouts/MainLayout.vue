<template>
  <div>
    <Loader v-if="loading"/>
    <div class="app-main-layout" v-else>
        <main class="app-content" :class='{full: !isOpen}'>
            <Navbar @click="isOpen = !isOpen" />
            <Sidebar v-model="isOpen" :key="locale"/>
            <div class="app-page">
                <router-view />
            </div>
        </main>

        <div class="fixed-action-btn">
            <router-link class="btn-floating btn-large blue" to="/record" v-tooltip="'Создать новую запись'">
                <i class="large material-icons">add</i>
            </router-link>
        </div>
    </div>
  </div>
</template>

<script>

import Navbar from '@/components/app/Navbar'
import Sidebar from '@/components/app/Sidebar'
import messages from '@/comand/messages'

export default {
  name: 'main-layout',
  computed: {
    error () {
      return this.$store.getters.error
    },
    locale () {
      return this.$store.getters.info.locale
    }
  },
  watch: {
    error (fbError) {
      this.$error(messages[fbError.code] || 'что-то пошло не так')
    }
  },
  data: () => ({
    isOpen: true,
    loading: true
  }),
  async mounted () {
    if (!Object.keys(this.$store.getters.info).length) {
      await this.$store.dispatch('fetchInfo')
    }

    this.loading = false
  },
  components: {
    Navbar, Sidebar
  }
}
</script>
