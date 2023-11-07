<template>
  <v-btn @click="toggleTheme" class="themeToggler">
    {{ themeIcon }}
  </v-btn>
</template>

<script setup>
  import { ref, onMounted } from 'vue'
  import { useTheme } from 'vuetify'

  const theme = useTheme()
  let themeIcon = ''

  function toggleTheme () {
    if (theme.global.current.value.dark) {
      theme.global.name.value = 'light'
      themeIcon = '🌚'
    } else {
      theme.global.name.value = 'dark'
      themeIcon = '🌞'
    }

    console.log(themeIcon)
  }

  onMounted(() => {
    if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) toggleTheme()
  });
</script>

<style>

.themeToggler {
  position: absolute !important;
  bottom: 5dvh !important;
  right: calc(50% - 2rem);
  font-size: 4rem !important;
  line-height: 4rem !important;
  width: 4rem;
  height: 4rem !important;
  padding: 0 !important;
  margin: 0 !important;
  border-radius: 50% !important;
}

</style>