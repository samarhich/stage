<template>
    <div class="admin-container">
      <!-- Sidebar -->
      <SideBar @changeView="changeView" :view="view" />
  
      <!-- Main content -->
      <div class="main-content">
        <!-- Views -->
        <div v-if="view === 'Consulte'" class="view-container">
          <ListProduct @changeView="changeView" />
        </div>
  
        <div v-else-if="view === 'add'" class="view-container">
          <AddProduct @changeView="changeView" />
        </div>
  
        <div v-else-if="view === 'Commande'" class="view-container">
          <listCommande @changeView="changeView" />
        </div>
  
        <div v-else-if="view === 'user'" class="view-container">
          
          <ListUser />

        
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import ListProduct from "../../components/adminService/listProducts.vue";
  import AddProduct from "../../components/adminService/AddProduit.vue";
  import SideBar from "../../components/adminService/SideBar.vue";
  import ListUser from "@/components/adminService/listUser.vue";
  import listCommande from "@/components/adminService/listCommande.vue";
  
  export default {
    data() {
      return {
        view: "Consulte",
      };
    },
    methods: {
      changeView(data) {
        this.view = data;
      },
    },
    components: {
      SideBar,
      AddProduct,
      ListProduct,
      ListUser,
      listCommande,
    },
  };
  </script>
  
  <style scoped>
  /* Global Styles */
  .admin-container {
    display: flex;
    height: 100vh;
    background-color: #f8f9fa;
  }
  
  /* Sidebar Styling */
  .sidebar {
    width: 250px;
    background-color: #343a40;
    color: #fff;
    padding: 20px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    height: 100vh;
  }
  
  .sidebar .menu-item {
    padding: 10px 20px;
    margin-bottom: 10px;
    border-radius: 4px;
    transition: background-color 0.3s;
  }
  
  .sidebar .menu-item:hover {
    background-color: #007bff;
  }
  
  /* Main content area */
  .main-content {
    flex-grow: 1;
    padding: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #e9ecef;
  }
  
  .view-container {
    width: 100%;
    max-width: 1200px;
    padding: 20px;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(49, 28, 28, 0.1);
    transition: all 0.3s ease;
  }
  
  .view-container h2 {
    color: #333;
    margin-bottom: 20px;
  }
  
  .view-container .card {
    background-color: #f1f1f1;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 20px;
  }
  
  .view-container .card:hover {
    box-shadow: 0 4px 8px rgba(45, 33, 33, 0.2);
  }
  
  /* Add some responsiveness */
  @media (max-width: 768px) {
    .admin-container {
      flex-direction: column;
    }
  
    .main-content {
      padding: 10px;
    }
  
    .view-container {
      max-width: 100%;
      padding: 10px;
    }
  }
  </style>
  