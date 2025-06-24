<template>
  <!-- menambahkan admin -->
  <div class="container mt-5">
    <h2>Admin Dashboard</h2>

    <!-- Form Tambah -->
    <form @submit.prevent="addBouquet" class="mb-4">
      <input v-model="form.name" placeholder="Name" class="form-control mb-2" required />
      <textarea v-model="form.description" placeholder="Description" class="form-control mb-2"></textarea>
      <input v-model.number="form.price" type="number" placeholder="Price" class="form-control mb-2" required />
      <button class="btn btn-success">Add Bouquet</button>
    </form>

    <!-- List Bouquet -->
    <ul class="list-group">
      <li
        v-for="b in bouquets"
        :key="b.id"
        class="list-group-item"
      >
        <!-- Mode Edit -->
        <div v-if="editId === b.id">
          <input v-model="editForm.name" class="form-control mb-1" />
          <textarea v-model="editForm.description" class="form-control mb-1"></textarea>
          <input v-model.number="editForm.price" type="number" class="form-control mb-2" />
          <button class="btn btn-primary btn-sm me-2" @click="saveEdit(b.id)">Save</button>
          <button class="btn btn-secondary btn-sm" @click="cancelEdit">Cancel</button>
        </div>

        <!-- Mode Tampilkan -->
        <div v-else class="d-flex justify-content-between align-items-center">
          <div>
            <strong>{{ b.name }}</strong> - {{ b.price }}<br />
            <small>{{ b.description }}</small>
          </div>
          <div>
            <button class="btn btn-warning btn-sm me-2" @click="startEdit(b)">Edit</button>
            <button class="btn btn-danger btn-sm" @click="deleteBouquet(b.id)">Delete</button>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
const BASE_URL = "https://backend-saya.nevameiy.workers.dev/api/bouquets";

export default {
  data() {
    return {
      bouquets: [],
      form: {
        name: "",
        description: "",
        price: ""
      },
      editId: null,
      editForm: {
        name: "",
        description: "",
        price: ""
      }
    };
  },
  async created() {
    const res = await axios.get(BASE_URL);
    this.bouquets = res.data;
  },
  methods: {
    async addBouquet() {
      await axios.post(BASE_URL, this.form);
      this.form = { name: "", description: "", price: "" }; // method untuk menambahkan data
      this.refreshData();
    },
    async deleteBouquet(id) {
      await axios.delete(`${BASE_URL}/${id}`); //method untuk delate data
      this.bouquets = this.bouquets.filter(b => b.id !== id);
    },
    startEdit(bouquet) {
      this.editId = bouquet.id;
      this.editForm = { ...bouquet };
    },
    cancelEdit() {
      this.editId = null;
      this.editForm = { name: "", description: "", price: "" };
    },
    async saveEdit(id) {
      await axios.put(`${BASE_URL}/${id}`, this.editForm);
      this.editId = null;
      this.editForm = { name: "", description: "", price: "" };
      this.refreshData();
    },
    async refreshData() {
      const res = await axios.get(BASE_URL);
      this.bouquets = res.data;
    } //methhod untuk edit/update data
  }
};
</script>
