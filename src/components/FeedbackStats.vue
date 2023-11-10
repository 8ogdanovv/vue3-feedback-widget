<template>
  <div class="flex align-center">
    <v-card class="card-container d-flex flex-column mt-1" elevation="10">
      <div class="d-flex justify-center text-h5">
        Rating overview
      </div>

      <div class="d-flex align-center flex-column">
        <div class="text-h3 mt-2">
          {{ computedRating }}
          <span class="text-h6 ml-n3">/5</span>
        </div>

        <v-rating
          :model-value="computedRating"
          color="yellow-darken-3"
          half-increments
          readonly
        ></v-rating>

        <div class="px-3">{{ feedbackData.length }} ratings</div>
      </div>

      <v-list bg-color="transparent" class="d-flex flex-column-reverse" density="compact">
        <v-list-item v-for="item in ratingDistribution" :key="item.rating">
          <v-progress-linear
            :model-value="item.percentage"
            class="mx-n5"
            color="yellow-darken-3"
            height="16"
            rounded
          ></v-progress-linear>

          <template v-slot:prepend>
            <span>{{ item.rating }}</span>
            <v-icon icon="mdi-star" class="mx-3"></v-icon>
          </template>

          <template v-slot:append>
            <div class="rating-values">
              <span class="d-flex justify-end">{{ item.count }}</span>
            </div>
          </template>
        </v-list-item>
      </v-list>
    </v-card>
  </div>
</template>

<script setup>
import { toRefs, computed } from 'vue';

const { feedbackData } = toRefs(defineProps({ feedbackData: Array }));

const computedRating = computed(() => {
  const totalRatings = feedbackData.value.length;
  if (totalRatings === 0) return 0;

  const totalSum = feedbackData.value.reduce((acc, cur) => acc + Number(cur.rating), 0);
  return Math.round((totalSum / totalRatings) * 2) / 2;
});

const ratingDistribution = computed(() => {
  const distribution = Array.from({ length: 5 }, (_, i) => i + 1).map((rating) => {
    const count = feedbackData.value.filter((item) => item.rating === rating).length;
    const percentage = (count / feedbackData.value.length) * 100;
    return { rating, count, percentage };
  });

  return distribution;
});
</script>

<style>
.card-container {
  width: 60dvw;
  height: 50dvh;
}

@media (max-width: 768px), (max-height: 1080px) {
  .card-container {
    height: 60dvh;
    min-height: 385px;
    min-width: 250px;
  }
}
</style>
