<template>
  <div class="container py-5">
    <p class="fs-3 fw-bold text-center text-primary mb-4">Liste des Personnes</p>
    
    <!-- Table displaying users -->
    <div class="table-responsive">
      <table class="table table-striped table-bordered shadow-sm">
        <thead class="bg-primary text-white">
          <tr>
            <th class="text-center">ID</th>
            <th class="text-center">Image</th>
            <th class="text-center">Nom</th>
            <th class="text-center">Prénom</th>
            <th class="text-center">Numéro de Téléphone</th>
            <th class="text-center">Actions</th>
          </tr>
        </thead>
        <tbody v-if="users.length === 0">
          <tr>
            <td class="text-center" colspan="6">Aucune donnée trouvée</td>
          </tr>
        </tbody>
        <tbody v-else>
          <tr v-for="user in users" :key="user.id">
            <td class="text-center">{{ user.id }}</td>
            <td class="text-center">
              <img :src="'http://localhost:8000' + user.image" alt="user-image" width="100px" class="img-fluid">
            </td>
            <td class="text-center">{{ user.nom }}</td>
            <td class="text-center">{{ user.prenom }}</td>
            <td class="text-center">{{ user.num_tlf }}</td>
            <td class="text-center">
              <button class="btn btn-danger mx-2" @click="DeleteUser(user.id)">Supprimer</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import UserService from "@/service/user_service/UserService.js";

export default {
  name: "ListView",
  created() {
    this.getUsers();
  },
  data() {
    return {
      users: []
    };
  },
  methods: {
    getUsers() {
      UserService.getUsers().then((res) => {
        this.users = res.data.data;
      });
    },
    DeleteUser(id) {
      UserService.DeletUser(id).then(() => {
        this.getUsers();
      });
    }
  }
};
</script>

<style scoped>
.container {
  max-width: 1200px;
  margin: 0 auto;
}

.table {
  border-radius: 10px;
  background-color: #ffffff;
}

th, td {
  vertical-align: middle;
}

th {
  background-color: #007bff;
  color: rgb(255, 255, 255);
  font-weight: bold;
}

tbody tr:hover {
  background-color: #f1f1f1;
  cursor: pointer;
}

img {
  width: 100px; /* Image size fixed */
  height: 100px; /* Fixed height for square shape */
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

.text-primary {
  color: #007bff !important;
}

.table-responsive {
  max-height: 500px;
  overflow-y: auto;
}
</style>
s