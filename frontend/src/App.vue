<template>
  <div class="p-6 max-w-2xl mx-auto bg-gray-50 rounded-lg shadow-lg">
    <div class="text-center">
      <img
        src="./assets/shirt.webp"
        alt="Produk Baju"
        class="w-full max-h-96 object-cover rounded-lg"
      />
      <h2 class="text-2xl font-bold my-4">Baju Kasual Pria/Wanita</h2>
      <p class="text-gray-600">Harga : Rp. 75.000</p>
      <p class="text-gray-600">Baju berkualitas tinggi, nyaman dipakai sehari-hari.</p>
      <button
        @click="fetchSentiment"
        :disabled="loading"
        class="mt-4 px-4 py-2 bg-blue-500 text-white rounded-lg disabled:bg-gray-400"
      >
        {{ loading ? "Checking..." : "AI Sentiment Check" }}
      </button>
    </div>
    <h3 class="text-xl font-semibold mt-6">Customer Reviews</h3>
    <ReviewList v-if="reviews.length" :reviews="reviews" />
    <p v-else class="text-gray-500 text-center mt-4">Belum ada review.</p>
  </div>
</template>

<script>
import { ref } from "vue";
import ReviewList from "./components/ReviewList.vue";

export default {
  components: { ReviewList },
  setup() {
    const reviews = ref([
    { name: "Andi", review: "Baju ini sangat nyaman dan berkualitas.", "sentiment": null },
    { name: "Siti", review: "Warnanya jelek bagus dan bahannya agak tipis.", "sentiment": null },
    { name: "Budi", review: "Sangat sesuai dengan ekspektasi saya!", "sentiment": null },
    { name: "Rina", review: "Modelnya kekinian, tapi bahannya kurang nyaman.", "sentiment": null },
    { name: "Dewi", review: "Harganya terjangkau dan kualitasnya oke.", "sentiment": null },
    { name: "Joko", review: "Bajunya robek setelah dicuci sekali.", "sentiment": null },
    { name: "Lisa", review: "Warnanya sangat cantik dan sesuai gambar!", "sentiment": null },
    { name: "Fajar", review: "Saya kecewa karena ukurannya tidak sesuai.", "sentiment": null },
    { name: "Rahmat", review: "Bahan adem dan nyaman dipakai seharian.", "sentiment": null },
    { name: "Sari", review: "Pengiriman cepat dan produknya bagus!", "sentiment": null },
    { name: "Dian", review: "Ukuran terlalu kecil dibandingkan standar.", "sentiment": null },
    { name: "Hendra", review: "Desainnya menarik, tapi bahannya kurang bagus.", "sentiment": null },
    { name: "Mila", review: "Saya suka sekali! Akan beli lagi.", "sentiment": null },
    { name: "Bayu", review: "Kurang puas karena warna tidak sesuai foto.", "sentiment": null },
    { name: "Nina", review: "Jahitannya rapi dan kualitas premium.", "sentiment": null },
    { name: "Tono", review: "Biasa saja, tidak ada yang istimewa.", "sentiment": null },
    { name: "Vina", review: "Bajunya sangat nyaman dan pas di badan.", "sentiment": null },
    { name: "Arif", review: "Bahannya mudah kusut dan kurang nyaman.", "sentiment": null },
    { name: "Siska", review: "Suka banget! Bahannya lembut dan adem.", "sentiment": null },
    { name: "Yoga", review: "Harga mahal tapi kualitas tidak sebanding.", "sentiment": null }
    ]);
    const loading = ref(false);

    const fetchSentiment = async () => {
      loading.value = true;
      const updatedReviews = await Promise.all(
        reviews.value.map(async (review) => {
          try {
            const response = await fetch("http://localhost:8000/predict", {
              method: "POST",
              headers: { "Content-Type": "application/json" },
              body: JSON.stringify({ review: review.review })
            });
            const data = await response.json();
            return { ...review, sentiment: data.sentimen };
          } catch (error) {
            console.error("Error fetching sentiment:", error);
            return { ...review, sentiment: "unknown" };
          }
        })
      );
      reviews.value = updatedReviews;
      loading.value = false;
    };

    return { reviews, loading, fetchSentiment };
  }
};
</script>
