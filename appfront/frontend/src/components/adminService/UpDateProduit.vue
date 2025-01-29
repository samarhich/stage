<template>
    <div class="container py-5">
      <div class="card shadow-lg p-4 mb-4">
        <h3 class="text-center text-primary mb-4">Mise à jour du produit</h3>
      </div>
  
      <form @submit.prevent="UpdateProudct()" method="POST" enctype="multipart/form-data">
        <input type="hidden" name="id">
        
        <div class="mb-4">
          <label for="productName" class="form-label">Nom du produit</label>
          <input
            type="text"
            id="productName"
            v-model="nom"
            class="form-control"
            placeholder="Entrez le nom du produit"
          />
        </div>
  
        <div class="mb-4">
          <label for="productPrice" class="form-label">Prix du produit</label>
          <input
            type="number"
            id="productPrice"
            v-model="prix"
            class="form-control"
            placeholder="Entrez le prix du produit"
          />
        </div>
  
        <div class="mb-4">
          <label for="productImage" class="form-label">Image du produit</label><br>
          <img
            :src="'http://localhost:8000'+image"
            alt="Product Image"
            class="img-thumbnail mb-3"
            style="width: 100px; height: 100px; object-fit: cover;"
          />
          <input
            type="file"
            name="photo"
            ref="photo"
            id="productImage"
            @change="UploadPhoto"
            class="form-control"
          />
        </div>
  
        <button type="submit" class="btn btn-warning w-100 mt-3">Mettre à jour</button>
      </form>
    </div>
  </template>
  
  <script>
  import ProductService from "@/service/product_service/ProductService.js";
  
  export default {
    props: {
      id: Number,
    },
    created() {
      this.getProductById();
    },
    data() {
      return {
        nom: "",
        prix: "",
        image: "",
        upload: 0,
      };
    },
    methods: {
      UploadPhoto() {
        this.image = this.$refs.photo.files[0];
        this.upload = 1;
      },
      UpdateProudct() {
        ProductService.UpdateProudct(
          {
            nom: this.nom,
            prix: this.prix,
            image: this.image,
            upload: this.upload,
          },
          this.id
        ).then((res) => {
          this.$router.go();
        });
      },
      getProductById() {
        ProductService.getProductById(this.id).then((res) => {
          this.nom = res.data.data['nom'];
          this.prix = res.data.data['prix'];
          this.image = res.data.data['image'];
        });
      },
    },
  };
  </script>
  
  <style scoped>
  .container {
    max-width: 600px;
  }
  
  .card {
    border-radius: 10px;
  }
  
  .form-label {
    font-size: 1rem;
    font-weight: 600;
  }
  
  .form-control {
    font-size: 1rem;
  }
  
  .img-thumbnail {
    border-radius: 10px;
    transition: transform 0.3s ease;
  }
  
  .img-thumbnail:hover {
    transform: scale(1.1);
  }
  
  button {
    font-size: 1.1rem;
    font-weight: bold;
  }
  </style>
  