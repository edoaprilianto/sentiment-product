<template>
    <div class="mt-6">
        <!-- Ringkasan Sentimen -->
    <div class="flex justify-between bg-gray-100 p-4 rounded-lg shadow">
      <div class="text-green-600 font-semibold flex items-center">
        Good ✅: {{ positiveCount }}
      </div>
      <div class="text-red-600 font-semibold flex items-center">
        Bad ❌: {{ negativeCount }}
      </div>
    </div>

      <table class="min-w-full bg-white border border-white-600 rounded-lg shadow-md">
        <thead class="bg-gray-200 text-gray-700">
          <tr>
            <th class="py-2 px-4 border-b">Nama</th>
            <th class="py-2 px-4 border-b">Review</th>
            <th class="py-2 px-4 border-b">Sentimen</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(review, index) in reviews" :key="index" class="text-center">
            <td class="py-2 px-4 border-b">{{ review.name }}</td>
            <td class="py-2 px-4 border-b">{{ review.review }}</td>
            <td class="py-2 px-4 border-b">
              <span v-if="review.sentiment === 'positive'" class="text-green-600 font-semibold flex items-center justify-center">
                Good ✅
              </span>
              <span v-else-if="review.sentiment === 'negative'" class="text-red-600 font-semibold flex items-center justify-center">
                Bad ❌
              </span>
              <span v-else class="text-gray-500">Belum dicek</span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
import { computed } from "vue";

export default {
  props: {
    reviews: {
      type: Array,
      default: () => []
    }
  },
  setup(props) {
    // Menghitung jumlah positif dan negatif
    const positiveCount = computed(() => props.reviews.filter(r => r.sentiment === "positive").length);
    const negativeCount = computed(() => props.reviews.filter(r => r.sentiment === "negative").length);

    return { positiveCount, negativeCount };
  }
};
</script>