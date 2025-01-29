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
        <h1 class="h4 text-gray-900">Bienvenue </h1>
      </div>

      <form @submit.prevent="Register()" enctype="multipart/form-data">
        <div class="mb-3">
          <input
            type="text"
            placeholder="Nom..."
            v-model="nom"
            class="form-control form-control-lg rounded-pill p-3"
            :class="{'is-invalid': errors.nom}"
            id="exampleInputNom"
          />
          <div v-if="errors.nom" class="invalid-feedback">{{ errors.nom }}</div>
        </div>
        <div class="mb-3">
          <input
            type="text"
            placeholder="Prénom..."
            v-model="prenom"
            class="form-control form-control-lg rounded-pill p-3"
            :class="{'is-invalid': errors.prenom}"
            id="exampleInputPrenom"
          />
          <div v-if="errors.prenom" class="invalid-feedback">{{ errors.prenom }}</div>
        </div>
        <div class="mb-3">
          <input
            type="email"
            placeholder="Adresse email..."
            v-model="email"
            class="form-control form-control-lg rounded-pill p-3"
            :class="{'is-invalid': errors.email}"
            id="exampleInputEmail1"
          />
          <div v-if="errors.email" class="invalid-feedback">{{ errors.email }}</div>
        </div>
        <div class="mb-3">
          <input
            type="password"
            placeholder="Mot de passe"
            v-model="password"
            class="form-control form-control-lg rounded-pill p-3"
            :class="{'is-invalid': errors.password}"
            id="exampleInputPassword1"
          />
          <div v-if="errors.password" class="invalid-feedback">{{ errors.password }}</div>
        </div>
        <div class="mb-4">
          <input
            type="text"
            placeholder="Numéro de téléphone..."
            v-model="num_tlf"
            class="form-control form-control-lg rounded-pill p-3"
            :class="{'is-invalid': errors.num_tlf}"
            id="exampleInputPhone"
          />
          <div v-if="errors.num_tlf" class="invalid-feedback">{{ errors.num_tlf }}</div>
        </div>
        <div class="mb-4">
          <input
            type="file"
            ref="photo"
            @change="UploadPhoto"
            class="form-control form-control-lg"
            :class="{'is-invalid': errors.image}"
          />
          <div v-if="errors.image" class="invalid-feedback">{{ errors.image }}</div>
        </div>
        <button type="submit" class="btn btn-success btn-lg w-100 rounded-pill">
          S'inscrire
        </button>
      </form>
      <hr />
      <div class="text-center">
        <router-link to="/signin">Vous avez déjà un compte ? Connectez-vous !</router-link>
      </div>
    </div>
  </div>
</template>

<script>
import UserService from "@/service/user_service/UserService";

export default {
  created() {},
  data() {
    return {
      nom: "",
      prenom: "",
      email: "",
      password: "",
      num_tlf: "",
      image: "",
      errors: {
        nom: "",
        prenom: "",
        email: "",
        password: "",
        num_tlf: "",
        image: "",
      },
    };
  },
  methods: {
  UploadPhoto() {
    this.image = this.$refs.photo.files[0];
  },
  Register() {
    // Réinitialiser les erreurs
    this.errors = {
      nom: "",
      prenom: "",
      email: "",
      password: "",
      num_tlf: "",
      image: "",
    };

    // Vérification des champs obligatoires
    let isValid = true;

    if (!this.nom) {
      this.errors.nom = "Le nom est requis.";
      isValid = false;
    }

    if (!this.prenom) {
      this.errors.prenom = "Le prénom est requis.";
      isValid = false;
    }

    if (!this.email) {
      this.errors.email = "L'email est requis.";
      isValid = false;
    }

    if (!this.password) {
      this.errors.password = "Le mot de passe est requis.";
      isValid = false;
    } else {
      // Ajout de la validation du mot de passe
      const passwordRegex = /^(?=.*[0-9])(?=.*[!@#$%^&*(),.?":{}|<>])[A-Za-z0-9!@#$%^&*(),.?":{}|<>]{5,}$/;
      if (!passwordRegex.test(this.password)) {
        this.errors.password = "Le mot de passe doit contenir au moins 5 caractères, des chiffres et des symboles.";
        isValid = false;
      }
    }

    if (!this.num_tlf) {
      this.errors.num_tlf = "Le numéro de téléphone est requis.";
      isValid = false;
    }

    if (!this.image) {
      this.errors.image = "L'image est requise.";
      isValid = false;
    }

    // Si tous les champs sont valides, procéder à l'inscription
    if (isValid) {
      UserService.AddUser({
        nom: this.nom,
        prenom: this.prenom,
        email: this.email,
        password: this.password,
        num_tlf: this.num_tlf,
        image: this.image,
      }).then((res) => {
        this.$router.push({ name: "signin" });
      });
    }
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
.btn-success {
  background-color: #28a745;
  border-color: #28a745;
  font-size: 1.2rem;
  font-weight: bold;
}

.btn-success:hover {
  background-color: #218838;
  border-color: #1e7e34;
}

h1 {
  color: #28a745; /* Couleur du titre en vert */
}

hr {
  border-color: #28a745;
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
  background-color: rgba(13, 134, 40, 0.6);
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
