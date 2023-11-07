<template>
  <div class="app">
    <h1>
      <span class="product">{{ appTitle }}&nbsp;
        <span class="product-shadow">{{ appTitle }}&nbsp;</span>
      </span>

      Feedback App
    </h1>
    <template v-if="!formSubmitted">
      <FeedbackForm :feedbackData="feedbackData" @formSubmittedToTrue="handleFormSubmission" />
    </template>
    <template v-else>
      <FeedbackStats :feedbackData="feedbackData" />
    </template>

    <theme-toggler />
  </div>
</template>

<script setup>
  import FeedbackStats from '@/components/FeedbackStats.vue'
  import FeedbackForm from '@/components/FeedbackForm.vue'
  import ThemeToggler from '@/components/ThemeToggler.vue'

  const appTitle = import.meta.env.VITE_TITLE || 'Default Title'

  import { ref, onMounted } from 'vue';
  import { collection, getDocs } from 'firebase/firestore'
  import { db } from '@/firebase'

  const formSubmitted = ref(false);
  const feedbackData = ref([]);

  onMounted(async () => {
    const querySnapshot = await getDocs(collection(db, 'feedbacks'));
    const data = [];
    querySnapshot.forEach((doc) => {
      // doc.data() is never undefined for query doc snapshots
      feedbackData.value.push(doc.data());
    });
  });

  const handleFormSubmission = () => {
    formSubmitted.value = true;
  };
</script>

<style>
  .app {
    width: 100%;
    height: 95dvh;
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  h1 {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .app > *:not(.v-card) {
    flex-grow: 1;
  }

  .product {
    background: -webkit-linear-gradient(270deg, navy 30%, gold);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    color: transparent;
    font-weight: bold;
    font-size: 120%;
    text-align: center;
    position: relative;
    display: inline-flex;
  }

  .product-shadow {
    display: inline-flex;
    position: absolute;
    top: 0;
    left: 0;
    text-align: center;
    color: transparent;
    text-shadow: 0 0 3px var(--selection-colors-tx);
    opacity: 0.5;
  }
</style>