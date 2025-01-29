<template>
  <div class="d-flex flex-column flex-shrink-0 bg-light p-3" style="width: 280px; height: 100vh; box-shadow: 2px 0 10px rgba(0, 0, 0, 0.1);">
    <!-- Header with Admin name -->
    <div class="d-flex align-items-center mb-3">
      <a href="#" class="d-flex align-items-center mb-0 me-md-auto text-dark text-decoration-none">
        <i class="material-icons mx-2 text-primary">admin_panel_settings</i>
        <span class="fs-4 text-primary">Admin</span>
      </a>
    </div>

    <!-- Notification dropdown -->
    <div class="dropdown mb-3">
      <div class="notif position-relative" data-bs-toggle="dropdown" aria-expanded="false">
        <i class="material-icons mx-2 text-primary">notifications</i>
        <span class="ms-2 text-dark">Notifications</span> <!-- Added text for "Notifications" -->
        <span class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-danger">
          {{ Notifications.length }}
        </span>
      </div>
      <ul class="dropdown-menu Scroll_container" v-if="Notifications.length > 0">
        <li v-for="Notification in Notifications" :key="Notification.id">
          <a class="dropdown-item" href="#">{{ Notification.message }}</a>
        </li>
      </ul>
      <ul class="dropdown-menu" v-else>
        <li>
          <a class="dropdown-item" href="#">Aucune notification</a> <!-- Translated "No Notifications" -->
        </li>
      </ul>
    </div>

    <hr class="border-light">

    <!-- Sidebar navigation items -->
    <ul class="nav nav-pills flex-column mb-auto">
      <li class="nav-item">
        <a class="nav-link text-dark" :class="view === 'Consulte' ? 'active' : ''" @click="changeView('Consulte')">
          <i class="material-icons mx-2">category</i> Produits <!-- Translated "Products" -->
        </a>
      </li>
      <li class="nav-item">
        <a class="nav-link text-dark" :class="view === 'Commande' ? 'active' : ''" @click="changeView('Commande')">
          <i class="material-icons mx-2">add_shopping_cart</i> Commandes <!-- Translated "Orders" -->
        </a>
      </li>
      <li class="nav-item">
        <a class="nav-link text-dark" :class="view === 'user' ? 'active' : ''" @click="changeView('user')">
          <i class="material-icons mx-2">person</i> Utilisateurs <!-- Translated "Users" -->
        </a>
      </li>
<!--ajouté par moi samar -->
      <li class="nav-item">
        <a class="nav-link text-dark" :class="view === 'message' ? 'active' : ''" @click="changeView('message')">
          <i class="material-icons mx-2">message</i> Utilisateurs <!-- Translated "Users" -->
        </a>
      </li>
    </ul>

    <hr class="border-light">

    <!-- Logout button -->
    <button class="btn btn-danger mt-3 w-100" @click="logout">
      <i class="material-icons mx-2">logout</i> Déconnexion <!-- Translated "Logout" -->
    </button>
  </div>
</template>

<script>
import { AuthStore } from "../../store/index.js";
import NotifService from "../../service/user_service/UserService.js";

export default {
  created() {
    this.getNotification();
  },
  setup() {
    const store = AuthStore();
    return { store };
  },
  props: {
    view: String,
  },
  mounted() {
    window.Echo.channel('public').listen('NotifEvent', (e) => {
      this.getNotification();
    });
  },
  data() {
    return {
      Notifications: [],
    };
  },
  name: "DashboardView",
  methods: {
    getNotification() {
      NotifService.getNotif(this.store.user['id']).then((res) => {
        this.Notifications = res.data.data;
      });
    },
    changeView(data) {
      this.$emit("changeView", data);
    },
    logout() {
      this.store.logout();
      this.$router.push({ name: "signin" });
    },
  },
};
</script>

<style scoped>
/* Styling for sidebar elements */
li {
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.notif {
  cursor: pointer;
}

.Scroll_container {
  max-height: 200px;
  overflow-y: auto;
  scroll-behavior: smooth;
}

.nav-link.active {
  background-color: #0d6efd; /* Bootstrap primary blue */
  border-radius: 5px;
  color: white;
}

.nav-link:hover {
  background-color: #e9ecef; /* Light hover effect */
  border-radius: 5px;
}

hr {
  border-color: #d1d1d1;
}

/* Styling the notification button */
button {
  width: 100%;
  display: flex;
  justify-content: center;
  border-radius: 5px;
}

button i {
  margin-right: 10px;
}

/* Modify color and style for the sidebar background */
.bg-light {
  background-color: #f8f9fa !important;
  color: #343a40;
}

/* Additional styles for a modern, clean look */
.text-primary {
  color: #0d6efd !important;
}

.border-light {
  border-color: #d1d1d1 !important;
}

.material-icons {
  font-size: 20px;
}
</style>
