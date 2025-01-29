<template>
  <div>
    <!-- Navbar -->
    <nav class="navbar navbar-expand-lg bg-body-tertiary">
      <div class="container-fluid">
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <li class="nav-item">
              <a class="nav-link active" aria-current="page" href="#accueil">Accueil</a>
            </li>
            <li class="nav-item">
              <a class="nav-link active" aria-current="page" href="#services">Services</a>
            </li>
            <li class="nav-item">
              <a class="nav-link active" aria-current="page" href="#location">Localisation</a>
            </li>
            <li class="nav-item">
              <a class="nav-link active" aria-current="page" href="#contact">Contact</a>
            </li>
            <li class="nav-item dropdown">
              <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                Opportunité
              </a>
              <ul class="dropdown-menu">
                <li><a class="dropdown-item" href="#">Stages</a></li>
                <li><a class="dropdown-item" href="#">Formation</a></li>
                <li><hr class="dropdown-divider"></li>
              </ul>
            </li>
          </ul>
          <form class="d-flex" role="search">
            <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
            <button class="btn btn-outline-success" type="submit">Search</button>
          </form>
        </div>
      </div>
    </nav>

    <!-- Image d'arrière-plan et contenu centré -->
    <div class="background-container">
      <div class="overlay"></div>
      <div class="container d-flex justify-content-center align-items-center" style="position: absolute; top: 0; bottom: 0; left: 0; right: 0;">
        <div class="text-center text-white">
          <img src="@/assets/cimenterie.jpg" alt="Logo" class="logo mb-4">
          <h1 class="display-4">Les Ciments de Bizerte</h1>
          <p class="lead">Les Ciments de Bizerte est une société tunisienne fondée en 1952. Elle est spécialisée dans la production de clinker, de divers types de ciment et de chaux hydraulique.</p>
          <div class="mt-4">
            <router-link to="/signin" class="btn btn-success btn-lg">Connexion</router-link>
          </div>
          <div class="mt-3">
            <p>Vous n'avez pas de compte ? 
              <router-link to="/signup" class="text-white text-decoration-underline link-hover">Inscrivez-vous</router-link>
            </p>
          </div>
        </div>
      </div>
    </div>

    <!-- Section Présentation -->
    <div id="accueil" class="mt-5" style="padding: 60px 0; background-color: #f9f9f9;">
      <div class="container text-center">
        <h2 class="display-4">Présentation</h2>
        <p class="lead">Les Ciments de Bizerte (SCB) sont l'un des acteurs majeurs de l'industrie du ciment en Tunisie. Fondée en 1952, l'entreprise se distingue par son engagement envers la qualité et l'innovation.</p>
        <div class="mt-5">
          <h3>Les composantes de la SCB</h3>
          <ul class="list-unstyled mt-4">
            <li><strong>Usine de Production :</strong> Baie de Sabra, à 3 km Sud-Ouest de Bizerte, sur la route de la pêcherie.</li>
            <li><strong>Carrières :</strong> Situées à 2,5 km de l’usine à Bir Messiougha et Jebal Bakkar.</li>
            <li><strong>Bâtiment Administratif :</strong> Rue Habib Thameur, Ville de Bizerte.</li>
            <li><strong>Bureau de Tunis :</strong> Centre Urbain Nord, 1082 – Tunis.</li>
          </ul>
        </div>
      </div>
    </div>

    
  <!-- Section Produits Récents -->
  <div id="portfolio" class="container my-5">
    <h1 class="text-dark text-center mb-4">Derniers Produits</h1>

    <div class="container p-0">
      <div v-if="load" class="text-center">
        <div class="spinner-border text-primary" role="status">
          <span class="visually-hidden">Chargement...</span>
        </div>
      </div>

      <div class="row g-4">
        <!-- Boucle sur les produits -->
        <div class="col-lg-4 col-md-6 col-sm-12" v-for="prod in Product" :key="prod.id">
          <div class="card portfolio-box shadow-sm rounded">
            <img
              class="img-fluid rounded-top"
              width="100%"
              :src="'http://localhost:8000' + prod.image"
              alt="Image du produit"
            />
            <div class="card-body text-center">
              <h5 class="card-title">{{ prod.nom }}</h5>
              <p class="card-text">Prix: {{ prod.prix }} TND</p>
              <button class="btn btn-primary">Voir Détails</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>


    <!-- Section Services -->
    <div id="services" class="mt-5">
       <h1 class="text-dark text-center mb-4">Services</h1>
      <serviceCompo />
    </div>

    <!-- Section Localisation -->
    <section class="page-section text-white" id="location">
      <div class="map-container" style="height: 50vh;">
        <h1 class="text-dark text-center mb-4">Localisation géographique</h1>
        <p class="text-center mb-4">Découvrez notre localisation sur la carte ci-dessous.</p>

        <iframe 
          src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3174.775007038631!2d9.859289981929834!3d37.27676206880938!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x12e31f08aeb072a3%3A0x95f7f0a50ce2758c!2sLes%20Ciments%20de%20Bizerte!5e0!3m2!1sfr!2stn!4v1705950436445!5m2!1sfr!2stn" 
          width="100%" 
          height="100%" 
          style="border:0;" 
          allowfullscreen="" 
          loading="lazy" 
          referrerpolicy="no-referrer-when-downgrade">
        </iframe>
      </div>
    </section>

    <!-- Section Contact -->
    <div id="contact">
      <contactCompo />
    </div>

    <!-- Footer -->
    <footer class="bg-light py-5">
      <div class="container px-4 px-lg-5">
        <div class="small text-center text-muted">Copyright &copy; 2025 - Les Ciments De Bizerte</div>
      </div>
    </footer>
  </div>
</template>

<script>
import serviceCompo from '@/components/service.vue';
import contactCompo from '@/components/contact.vue';
import ProductService from '@/service/product_service/ProductService';

export default {
  name: 'HomePage',
  data() {
    return {
      load: true,
      Product: []
    };
  },
  created() {
    this.getproduct();
  },
  methods: {
    getproduct() {
      ProductService.getProudctLatest()
        .then((res) => {
          this.load = false;
          this.Product = res.data.data;
        })
        .catch((err) => {
          this.load = false;
          console.error('Erreur lors de la récupération des produits :', err);
        });
    }
  },
  components: {
    serviceCompo,
    contactCompo
  }
};
</script>

<style scoped>
/* Ajuster la taille des sections */
#services {
  padding: 60px 0;
  min-height: 730px;
}

#portfolio {
  padding: 60px 0;
}

#location {
  padding: 60px 0;
}

#contact {
  padding: 60px 0;
  background-color: #f9f9f9; /* Optionnel : couleur de fond claire pour le contact */
}

/* Ajuster la taille du fond */
.background-container {
  position: relative;
  height: 91.7vh; /* Ajuster la hauteur de l'arrière-plan */
  background-image: url('@/assets/imagehome2.jpg');
  background-size: cover;
  background-position: center;
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(93, 92, 163, 0.5);
  filter: blur(1px);
}

/* Styles des autres éléments */
.link-hover {
  transition: background-color 0.3s ease;
}

.link-hover:hover {
  background-color: #428a53;
  color: white;
  padding: 2px 10px;
  border-radius: 5px;
}

.logo {
  width: 220px;
  height: 150px;
  border-radius: 50%;
  object-fit: cover;
  margin-bottom: 1rem;
}

 #portfolio {
  padding: 1px ;
} 

h2 {
  font-size: 2.5rem;
  font-weight: bold;
}

.card {
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  cursor: pointer;
}

.card:hover {
  transform: scale(1.05);
  box-shadow: 0px 10px 30px rgba(0, 0, 0, 0.1);
}

.card-body {
  background-color: #fff;
}

.card-title {
  font-size: 1.2rem;
  font-weight: 600;
}

.card-text {
  font-size: 1rem;
  color: #555;
}

button {
  margin-top: 10px;
}

@media (max-width: 767px) {
  .card {
    margin-bottom: 20px;
  }
}

</style>    