<template>
  <div class="container py-5">
    <p class="fs-3 fw-bold text-center text-primary mb-4">Liste des Commandes</p>

    <div v-if="commandes.length === 0" class="text-center">
      <p>Aucune commande trouvée</p>
    </div>

    <div v-else class="row gap-4">
      <!-- Carte de commande -->
      <div v-for="commande in commandes" :key="commande.id" class="col-lg-4 col-md-6 col-sm-12">
        <div class="card shadow-sm border-0 rounded-3">
          <div class="card-body">
            <h5 class="card-title text-primary">
              Client : {{ commande.nom }} {{ commande.prenom }}
            </h5>
            <p class="card-text">
              <strong>Prix Total : </strong>{{ commande.prix_total }} TND
            </p>
            <div class="d-flex gap-2 justify-content-between">
              <button
                class="btn btn-danger"
                @click="RejectCommande(commande.id)"
              >
                Rejeter
              </button>
              <button
                class="btn btn-success"
                @click="AccepteCommande(commande.id)"
              >
                Accepter
              </button>
              <button
                type="button"
                class="btn btn-info"
                data-bs-toggle="modal"
                :data-bs-target="'#exampleModal' + commande.id"
              >
                Voir
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Modale de détail de commande -->
    <div
      v-for="commande in commandes"
      :key="'modal-' + commande.id"
      class="modal fade"
      :id="'exampleModal' + commande.id"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog modal-lg">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">
              Code Commande : {{ commande.code_commande }}
            </h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <h4 class="text-center text-primary mb-3">Info Ligne Commande</h4>
            <table class="table table-striped">
              <thead>
                <tr>
                  <th>Quantité</th>
                  <th>Prix par ligne</th>
                  <th>ID Produit</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="ligne in commande.lignecommande" :key="ligne.id">
                  <td>{{ ligne.quantite }}</td>
                  <td>{{ ligne.prix_ligne_commande }} TND</td>
                  <td>{{ ligne.produit_id }}</td>
                </tr>
              </tbody>
            </table>

            <h4 class="text-center text-primary mt-4">Info Client</h4>
            <table class="table">
              <thead>
                <tr>
                  <th>Nom</th>
                  <th>Prénom</th>
                  <th>Email</th>
                  <th>Ville</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td>{{ commande.nom }}</td>
                  <td>{{ commande.prenom }}</td>
                  <td>{{ commande.adresse_mail }}</td>
                  <td>{{ commande.ville }}</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
            >
              Fermer
            </button>
          </div>
        </div>
      </div>
    </div>

    <hr />
    <div class="mt-4">
      <Reject_Accepte_Commande />
    </div>
  </div>
</template>

<script>
import Reject_Accepte_Commande from "../../components/adminService/Reject_Accepte_Commande.vue";
import CommandeService from "../../service/Commande_service/CommandeService.js";
export default {
  name: "ListView",
  created() {
    this.getCommandes();
  },
  data() {
    return {
      commandes: [],
    };
  },
  methods: {
    getCommandes() {
      CommandeService.getCommandes(0).then((res) => {
        this.commandes = res.data.data;
      });
    },
    RejectCommande(id) {
      CommandeService.RejectCommande(id).then(() => {
        this.getCommandes();
      });
    },
    AccepteCommande(id) {
      CommandeService.AcceptCommande(id).then(() => {
        this.getCommandes();
      });
    },
  },
  components: {
    Reject_Accepte_Commande,
  },
};
</script>

<style scoped>
.card-body {
  padding: 20px;
}

.card-title {
  font-size: 1.25rem;
}

.card-text {
  font-size: 1rem;
}

.btn {
  font-size: 1rem;
  border-radius: 20px;
}

.modal-body {
  padding: 20px;
}

.table th, .table td {
  vertical-align: middle;
}

.table-striped tbody tr:nth-of-type(odd) {
  background-color: #f9f9f9;
}

.table th {
  background-color: #007bff;
  color: white;
}

.table td {
  font-size: 1rem;
}

.text-primary {
  color: #007bff;
}

.text-center {
  text-align: center;
}

h4 {
  font-size: 1.25rem;
  font-weight: bold;
}
</style>
