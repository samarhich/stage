<template>
    <div class="container py-5">
      <!-- Card for the form title -->
      <div class="card shadow-sm rounded-lg mb-4">
        <div class="card-body">
          <h3 class="text-center text-primary fw-bold">Ajouter un produit</h3>
        </div>
      </div>
  
      <!-- Form for adding product -->
      <form @submit.prevent="AddProduct()" enctype="multipart/form-data">
        <!-- Row for Name and Price inputs -->
        <div class="row mb-4">
          <!-- Product Name -->
          <div class="col-12 col-md-6">
            <label for="productName" class="form-label">Nom du produit</label>
            <input 
              type="text" 
              id="productName" 
              class="form-control form-control-lg" 
              v-model="nom" 
              placeholder="Entrez le nom du produit"
              required
            />
          </div>
  
          <!-- Product Price -->
          <div class="col-12 col-md-6">
            <label for="productPrice" class="form-label">Prix en DT</label>
            <input 
              type="number" 
              id="productPrice" 
              class="form-control form-control-lg" 
              v-model="prix" 
              placeholder="Entrez le prix du produit"
              min="0"
              required
            />
          </div>
        </div>
  
        <!-- Product Image Upload -->
        <div class="mb-4">
          <label for="productImage" class="form-label">Image du produit</label>
          <input 
            type="file" 
            id="productImage" 
            ref="photo" 
            @change="UploadFile" 
            class="form-control form-control-lg" 
            accept="image/*"
            required
          />
        </div>
  
        <!-- Submit Button -->
        <div class="text-center">
          <button type="submit" class="btn btn-success btn-lg px-5 py-3">Ajouter le produit</button>
        </div>
      </form>
    </div>
  </template>
  
  <script>
  import ProductService from "../../service/product_service/ProductService.js";
  
  export default {
    data() {
      return {
        nom: "",
        prix: "",
        image: "",
      };
    },
    methods: {
      AddProduct() {
        ProductService.AddProduct({
          nom: this.nom,
          prix: this.prix,
          image: this.image,
        }).then((res) => {
          this.$emit("changeView", "Consulte");
        });
      },
      UploadFile() {
        this.image = this.$refs.photo.files[0];
      },
    },
  };
  </script>
  
  <style scoped>
  /* Add custom spacing and rounded corners */
  .container {
    max-width: 800px;
    margin: 0 auto;
  }
  
  .card {
    border-radius: 15px;
  }
  
  .card-body {
    padding: 2rem;
  }
  
  h3 {
    font-size: 1.8rem;
    font-weight: bold;
    color: #007bff;
  }
  
  input[type="text"], input[type="number"], input[type="file"] {
    border-radius: 10px;
    padding: 15px;
    background-color: #f8f9fa;
    font-size: 1rem;
  }
  
  input:focus, button:focus {
    outline: none;
    box-shadow: 0 0 10px rgba(38, 143, 255, 0.5);
  }
  
  button {
    font-size: 1.25rem;
    border-radius: 30px;
    transition: background-color 0.3s;
  }
  
  button:hover {
    background-color: #28a745;
  }
  
  .form-control-lg {
    height: 50px;
  }
  
  .text-center {
    text-align: center;
  }
  </style>
  