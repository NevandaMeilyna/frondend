<template>
  <!-- membuat halaman home -->
  <div>
    <!-- Navbar -->
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">Vaa_Bouquet</a>
      </div>
    </nav>

    <!-- Hero Section -->
    <section class="bg-light text-center d-flex flex-column justify-content-center align-items-center w-100" style="height: 70vh; width: 100vw;">
  <div class="w-100 px-4">
    <h1 class="display-4 fw-bold">Selamat datang di Toko Buket Kami</h1>
    <p class="lead">Buket bunga cantik yang dibuat dengan tangan untuk setiap kesempatan.</p>
  </div>
</section>

    <!-- Product List -->
    <div class="container-fluid mt-5 px-5">
      <div class="row justify-content-center">
        <div v-for="bouquet in bouquets" :key="bouquet.id" class="col-sm-6 col-md-4 col-lg-3 mb-4">
          <div class="card h-100 shadow-sm">
            <div class="card-body d-flex flex-column">
              <h5 class="card-title">{{ bouquet.name }}</h5>
              <p class="card-text">{{ bouquet.description }}</p>
              <p class="fw-bold text-success mt-auto">{{ formatPrice(bouquet.price) }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Footer -->
    <footer class="bg-dark text-white text-center py-3 mt-5">
      <div class="container-fluid">
        <p class="mb-0">© 2025 Vaa_Bouquet Shop.</p>
      </div>
    </footer>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      bouquets: [],
    };
  },
  async created() {
    const res = await axios.get("https://backend-saya.nevameiy.workers.dev/api/bouquets");
    this.bouquets = res.data;
  },
  methods: {
    formatPrice(value) {
      return new Intl.NumberFormat("id-ID", {
        style: "currency",
        currency: "IDR",
        minimumFractionDigits: 0,
      }).format(value);
    }
  }
};
</script>

<style>
html, body, #app {
  width: 100%;
  margin: 0;
  padding: 0;
  overflow-x: hidden;
  background-color: #f8f9fa;
}
</style>

