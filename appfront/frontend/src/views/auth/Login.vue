<template>
  <div class="signup-container">
    <!-- Logo à gauche -->
    <div class="logo">
      <img src="@/assets/cimenterie.jpg" alt="Logo" class="logo-image" />
    </div>

    <!-- Lien de retour avec la flèche en haut à droite -->
    <div class="back-to-home">
      <router-link to="/" class="back-link">
        <i class="fa fa-arrow-left"></i> Retour à l'accueil
      </router-link>
    </div>

    <div class="form-wrapper">
      <div class="text-center mb-4">
        <h1 class="h4 text-gray-900 mb-5">Bienvenue</h1>
      </div>

      <form @submit.prevent="SignIn()">
        <div class="mb-4">
          <input
            type="text"
            placeholder="Email Address..."
            name="email"
            class="form-control rounded-pill p-4"
            v-model="email"
            id="exampleInputEmail1"
            :class="{'is-invalid': emailError}"
          />
          <div v-if="emailError" class="invalid-feedback">{{ emailError }}</div>
        </div>
        <div class="mb-5">
          <input
            type="password"
            placeholder="Password"
            name="password"
            class="form-control rounded-pill p-4"
            v-model="password"
            id="exampleInputPassword1"
            :class="{'is-invalid': passwordError}"
          />
          <div v-if="passwordError" class="invalid-feedback">{{ passwordError }}</div>
        </div>
        <button
          style="font-size:19px"
          type="submit"
          name="submit"
          class="btn btn-primary btn-block w-100 rounded-pill"
        >
          Connexion
        </button>
      </form>
      <hr />
      <div class="text-center">
        <router-link to="/signup">Créer un compte</router-link>
      </div>
    </div>
  </div>
</template>

<script>
import LoginService from "@/service/login_service/LoginService";
import { AuthStore } from "../../store/index";

export default {
  created() {},
  setup() {
    const store = AuthStore();
    return { store };
  },
  data() {
    return {
      email: "",
      password: "",
      emailError: "",
      passwordError: "",
    };
  },
  methods: {
    validateForm() {
      this.emailError = "";
      this.passwordError = "";

      // Validation de l'email
      if (!this.email) {
        this.emailError = "L'email est requis.";
      } else if (!this.validEmail(this.email)) {
        this.emailError = "L'email n'est pas valide.";
      }

      // Validation du mot de passe
      if (!this.password) {
        this.passwordError = "Le mot de passe est requis.";
      }

      // Retourner false si il y a une erreur, sinon true
      return !(this.emailError || this.passwordError);
    },

    validEmail(email) {
      const emailPattern = /^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,6}$/;
      return emailPattern.test(email);
    },

    SignIn() {
      // Validation du formulaire
      if (!this.validateForm()) {
        return; // Ne pas envoyer la demande si les champs ne sont pas valides
      }

      LoginService.signInUser(this.email, this.password)
        .then(() => {
          if (this.store.getisadmin == 1) {
            this.$router.push({ name: "DashboardAdmin" });
          } else {
            this.$router.push("/menu");
          }
        })
        .catch((error) => {
          console.log(error);
          this.passwordError = "Identifiants incorrects. Veuillez réessayer.";
        });
    },
  },
};
</script>

<style scoped>
/* Arrière-plan avec l'image et superposition */
.signup-container {
  background-image: url('@/assets/imagehome2.jpg'); /* Changez l'URL vers l'image souhaitée */
  background-size: cover;
  background-position: center;
  min-height: 100vh;
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
}

/* Effet de flou et superposition avec couleur */
.signup-container::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(93, 92, 163, 0.5); /* Couleur de la superposition avec transparence */
  filter: blur(2px); /* Applique un flou */
  z-index: 1;
}

/* Boîte du formulaire avec fond blanc transparent */
.form-wrapper {
  background-color: rgba(255, 255, 255, 0.9); /* Fond blanc avec transparence */
  padding: 2rem;
  border-radius: 15px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.2);
  position: relative;
  z-index: 2;
  max-width: 500px; /* Réduction de la taille de la boîte */
  width: 100%; /* Assure que la boîte ne dépasse pas */
}

/* Customisation du bouton */
.btn-primary {
  background-color: #28a745; /* Couleur verte */
  border-color: #28a745; /* Couleur verte */
  font-size: 1.2rem;
  font-weight: bold;
}

.btn-primary:hover {
  background-color: #218838; /* Teinte plus foncée de vert */
  border-color: #1e7e34; /* Teinte plus foncée de vert */
}

h1 {
  color: #28a745; /* Couleur du titre en vert */
}

hr {
  border-color: #28a745; /* Couleur verte pour la ligne */
}

/* Styles pour le logo à gauche */
.logo {
  position: absolute;
  top: 30px;
  left: 20px;
  z-index: 10;
}

.logo-image {
  width: 150px;
  height: auto;
  border-radius: 50%; /* Logo avec bords arrondis */
}

/* Styles pour le lien de retour en haut à droite */
.back-to-home {
  position: absolute;
  top: 45px;
  right: 30px;
  z-index: 10;
}
.back-link {
  color: #ffffff;
  font-size: 18px;
  font-weight: bold;
  text-decoration: none;
  background-color: rgba(13, 134, 40, 0.6); /* Couleur de fond verte */
  padding: 10px 15px;
  border-radius: 25px;
}

.back-link:hover {
  background-color: rgba(0, 0, 0, 0.8);
}

.back-link i {
  margin-right: 15px; /* Espace entre la flèche et le texte */
}

/* Styles pour les champs avec erreurs */
.is-invalid {
  border-color: #dc3545; /* Bordure rouge pour les champs invalides */
}

.invalid-feedback {
  color: #dc3545; /* Texte d'erreur en rouge */
  font-size: 0.875rem;
}
</style>
