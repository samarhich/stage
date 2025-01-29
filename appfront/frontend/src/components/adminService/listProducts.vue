<template>
  <div class="container py-5">
    <!-- If editing a product, show the update component -->
    <div v-if="product_id != null">
      <UpDateProduit :id="product_id"></UpDateProduit>
    </div>

    <!-- If no product is being edited, display the list -->
    <div v-else>
      <p class="text-center fs-3 fw-bold text-primary mb-4">Liste des Produits</p>
      
      <!-- Add product button -->
      <div class="text-center mb-4">
        <button @click="changeView('add')" class="btn btn-primary btn-lg px-5 py-3">Ajouter un produit</button>
      </div>

      <!-- Table displaying products -->
      <div class="table-responsive">
        <table class="table table-striped table-bordered shadow-sm">
          <thead>
            <tr>
              <th class="text-center">ID</th>
              <th class="text-center">Nom</th>
              <th class="text-center">Prix</th>
              <th class="text-center">Image</th>
              <th class="text-center">Actions</th>
            </tr>
          </thead>
          <tbody v-if="products.length === 0">
            <tr>
              <td class="text-center" colspan="5">Aucune donnée trouvée</td>
            </tr>
          </tbody>
          <tbody v-else>
            <tr v-for="product in products" :key="product.id">
              <td class="text-center">{{ product.id }}</td>
              <td class="text-center">{{ product.nom }}</td>
              <td class="text-center">{{ product.prix }} TND</td>
              <td class="text-center">
                <img :src="'http://localhost:8000'+product.image" alt="" width="100px" class="img-fluid">
              </td>
              <td class="text-center">
                <button class="btn btn-danger mx-2" @click="Deletproduct(product.id)">Supprimer</button>
                <button class="btn btn-warning" @click="edit(product.id)">Modifier</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import ProductService from "@/service/product_service/ProductService.js";
import UpDateProduit from "./UpDateProduit.vue";

export default {
  name: "ListView",
  created() {
    this.getProducts();
  },
  data() {
    return {
      products: [],
      product_id: null,
    };
  },
  methods: {
    changeView(data) {
      this.$emit("changeView", data);
    },
    getProducts() {
      ProductService.getProducts().then((res) => {
        this.products = res.data.data;
      });
    },
    Deletproduct(id) {
      ProductService.DeletProduct(id).then((res) => {
        this.getProducts();
      });
    },
    edit(id) {
      this.product_id = id;
    },
  },
  components: {
    UpDateProduit,
  },
};
</script>

<style scoped>
.container {
  max-width: 1200px;
  margin: 0 auto;
}

.table {
  border-radius: 10px;
  background-color: #fff;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

th, td {
  text-align: center;
  vertical-align: middle;
}

th {
  background-color: #007bff;
  color: white;
  font-weight: bold;
}

tbody tr:hover {
  background-color: #f1f1f1;
  cursor: pointer;
}

img {
  border-radius: 8px;
}

button {
  font-size: 1rem;
  border-radius: 20px;
  transition: background-color 0.3s;
}

button:hover {
  opacity: 0.8;
}

.text-center {
  text-align: center;
}

.fs-3 {
  font-size: 1.75rem;
}
</style>
