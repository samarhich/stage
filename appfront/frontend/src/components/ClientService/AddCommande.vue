<template>
  <div class="container">
    <div v-if="confirm == 0">
      <h2 class="text-center mb-4">Passer une Commande</h2>
      <form @submit.prevent="AddCommande()" class="form-container">
        <div class="form-group">
          <label for="nom">Nom</label>
          <input
            type="text"
            id="nom"
            v-model="nom"
            placeholder="Votre Nom"
            class="form-control"
          />
        </div>
        <div class="form-group">
          <label for="prenom">Prénom</label>
          <input
            type="text"
            id="prenom"
            v-model="prenom"
            placeholder="Votre Prénom"
            class="form-control"
          />
        </div>
        <div class="form-group">
          <label for="adresse_mail">Email</label>
          <input
            type="email"
            id="adresse_mail"
            v-model="adresse_mail"
            placeholder="Votre adresse email"
            class="form-control"
          />
        </div>
        <div class="form-group">
          <label for="ville">Ville</label>
          <input
            type="text"
            id="ville"
            v-model="ville"
            placeholder="Votre ville"
            class="form-control"
          />
        </div>
        <div class="form-actions">
          <button type="submit" class="btn btn-primary">Continuer</button>
          <button type="reset" class="btn btn-danger">Fermer</button>
        </div>
      </form>
    </div>

    <div v-else-if="confirm == 1" class="confirmation-card" style="max-width: 600px; margin: 0 auto; padding: 20px; border-radius: 10px; background-color: #fff; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);">
  <div class="card-header" style="background-color: #3f8bff; color: #fff; font-size: 1.2rem; text-align: center; padding: 15px; border-radius: 8px 8px 0 0;">
    Détails de la commande
  </div>
  <div class="card-body" style="font-size: 1rem; color: #333; padding: 25px; background-color: #f9f9f9; border-radius: 0 0 8px 8px;">
    <div class="mb-3">
      <h5 class="mb-1" style="font-weight: 500;">Nom: <span style="font-weight: 400; color: #555;">{{ nom }}</span></h5>
    </div>
    <div class="mb-3">
      <h5 class="mb-1" style="font-weight: 500;">Prénom: <span style="font-weight: 400; color: #555;">{{ prenom }}</span></h5>
    </div>
    <div class="mb-3">
      <h5 class="mb-1" style="font-weight: 500;">Email: <span style="font-weight: 400; color: #555;">{{ adresse_mail }}</span></h5>
    </div>
    <div class="mb-3">
      <h5 class="mb-1" style="font-weight: 500;">Ville: <span style="font-weight: 400; color: #555;">{{ ville }}</span></h5>
    </div>

    <div class="d-flex justify-content-between">
      <a @click="passCommande()" href="#" class="btn btn-success" style="padding: 10px 20px; font-size: 1rem; border-radius: 8px; background-color: #28a745; color: white; border: none;">
        Confirmer
      </a>
      <button @click="confirm = 0" class="btn btn-secondary" style="padding: 10px 20px; font-size: 1rem; border-radius: 8px; background-color: #6c757d; color: white; border: none;">
        Retour
      </button>
    </div>
  </div>
</div>


    <div v-else class="thank-you-message">
      <h3>Merci pour votre Commande !</h3>
      <p>Votre commande #{{ code_Commande }} a été enregistrée.</p>
      <p>Nous vous enverrons un email de confirmation avec les détails et le suivi de votre commande.</p>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import VueConfetti from 'vue-confetti'
Vue.use(VueConfetti)
import CommandeService from "../../service/Commande_service/CommandeService.js"
import {AuthStore} from "../../store/index.js"

export default {
  props: {
    prix_total: Number
  },
  setup() {
    const store = AuthStore();
    return { store }
  },
  data() {
    return {
      nom: "",
      prenom: "",
      adresse_mail: "",
      ville: "",
      confirm: false,
      code_Commande: ""
    };
  },
  methods: {
    passCommande() {
      let UserInfo = {
        nom: this.nom,
        prenom: this.prenom,
        ville: this.ville,
        adresse_mail: this.adresse_mail,
        userid: this.store.isauth != null ? this.store.user['id'] : null
      }
      this.code_Commande = Math.floor(Math.random() * 99999);

      CommandeService.AddCommande({
        user: UserInfo,
        code_Commande: this.code_Commande,
        product: JSON.stringify(JSON.parse(localStorage.getItem("products"))),
        prix_total: this.prix_total
      }).then((res) => {
        this.$confetti.start();
        localStorage.removeItem("products"); // Clear localStorage
        setTimeout(() => this.$confetti.stop(), 2000);
        setTimeout(() => this.$router.go(), 2500);
      })
    },
    AddCommande() {
      this.confirm = 1;
    }
  },
};
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

h2, h3 {
  color: #333;
}

.form-container {
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.form-group {
  margin-bottom: 1.5rem;
}

label {
  display: block;
  font-weight: bold;
  color: #555;
}

.form-control {
  width: 100%;
  padding: 12px;
  border-radius: 8px;
  border: 1px solid #ccc;
  box-sizing: border-box;
}

.form-actions {
  display: flex;
  justify-content: space-between;
}

.btn {
  padding: 12px 20px;
  border-radius: 6px;
  border: none;
  font-size: 16px;
}

.btn-primary {
  background-color: #007bff;
  color: white;
}

.btn-danger {
  background-color: #dc3545;
  color: white;
}

.btn-success {
  background-color: #28a745;
  color: white;
}

.btn-secondary {
  background-color: #6c757d;
  color: white;
}

.btn:hover {
  opacity: 0.9;
  cursor: pointer;
}

.confirmation-card {
  background-color: #f0f8ff;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.15);
}

.thank-you-message {
  text-align: center;
  padding: 20px;
  background-color: #e0f7fa;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.card-header {
  background-color: #007bff;
  color: white;
  padding: 10px;
  border-radius: 8px;
}

.card-body {
  margin: 20px 0;
}

.card-title {
  font-size: 1.2rem;
  margin: 10px 0;
}
</style>
