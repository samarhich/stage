<template>
  <div class="p-5" style="width: 100% !important; background-color: #f9f9f9;">
      <div class="d-flex justify-content-center mb-4">
          <div>
              <p class="titre text-center" style="font-size: 1.5rem; font-weight: 600; color: #333;">Liste des commandes</p>
          </div>
      </div>
    <div>
     <table class="table table-striped table-hover" style="background-color: #fff; border-radius: 8px;">
        <thead style="background-color: #3f8bff; color: white;">
           <tr>
              <th>#</th>
              <th>Nom</th>
              <th>Code de la Commande</th>
              <th>Prix Total</th>
              <th>Nombre de Produits</th>
              <th>Opérations</th>
           </tr>
        </thead>
        <tbody v-if="commandes==''">
          <tr>
              <td colspan="7" class="text-center" style="color: #999;">Pas de commande</td>
          </tr>
        </tbody>
        <tbody v-else>
          <tr :class="commande.status === 1 ? 'table-success' : commande.status === 2 ? 'table-danger' : 'table-warning'" 
              v-for="commande in commandes" :key="commande.id" 
              :style="{ backgroundColor: commande.status === 1 ? '#d4edda' : commande.status === 2 ? '#f8d7da' : '#fff3cd' }">
              <td>{{ commande.id }}</td>
               <td>{{ commande.nom }}</td>
               <td>{{ commande.code_commande }}</td>
               <td>{{ commande.prix_total }} DT</td>
               <td>{{ commande.lignecommande.length }}</td>
              <td>
                  <button class="btn btn-danger mx-2" @click="deleteCommande(commande.id)" style="padding: 5px 15px; font-size: 0.9rem; border-radius: 5px; transition: background-color 0.3s;">
                      Supprimer
                  </button>
                  <button v-if="commande.status == 1" class="btn btn-success" @click="DownloadPdf(commande)" style="padding: 5px 15px; font-size: 0.9rem; border-radius: 5px; transition: background-color 0.3s;">
                      Télécharger PDF
                  </button>
              </td>
          </tr>
        </tbody>
     </table>
    </div>
  </div>
</template>

<script>
import jsPDF from 'jspdf'
import autoTable from 'jspdf-autotable'
import { AuthStore } from "../../store/index.js"
import CommandeService from "../../service/Commande_service/CommandeService.js";

export default {
  name: "Reject_Accepte",
  setup() {
      const store = AuthStore();
      return { store }
  },
  created() {
      this.getCommandes();
  },
  mounted() {
      this.getCommandes();
  },
  data() {
    return {
      commandes: [],
    };
  },
  methods: {
    getCommandes() {
      CommandeService.getMyCommandes(this.store.user['id']).then((res) => {
        this.commandes = res.data.data;
        console.log(this.commandes);
      });
    },
    deleteCommande(id) {
      CommandeService.DeletCommande(id).then((res) => {
        this.getCommandes();
      });
    },

    DownloadPdf(commande) {
    const doc = new jsPDF();
    const logo = require('../../assets/cimenterie.jpg'); //  logo
    
    // Ajout du logo
    doc.addImage(logo, 'jpg', 10, 10, 30, 30);
    
    // Paramètres de style
    doc.setFont("times", "bold");
    doc.setFontSize(14);
    doc.text('BON DE COMMANDE', 90, 30, { align: 'center' });
    doc.setFontSize(10);
    doc.setFont("times", "normal");
    
    // Date et Code de la commande
    doc.text('Date : ' + commande.created_at.substr(0, 10), 158, 40);
    doc.text('Code Commande : ' + commande.code_commande, 158, 50);
    
    // Détails client
    doc.setFontSize(12);
    doc.text('Nom : ' + commande.nom, 15, 70);
    doc.text('Prénom : ' + commande.prenom, 15, 80);
    doc.text('Email : ' + commande.adresse_mail, 15, 90);
    doc.text('Ville : ' + commande.ville, 158, 70);
    
    // Séparation horizontale
    doc.setLineWidth(0.5);
    doc.line(10, 100, 200, 100);
    
    // Table des produits
    const header = ["Id", "Prix", "Quantité"];
    const body = commande.lignecommande.map(product => [
        product.produit_id,
        product.prix_ligne_commande,
        product.quantite
    ]);
    
    doc.setFontSize(11);
    doc.setFont("times", "normal");
    autoTable(doc, {
        head: [header],
        body: body,
        startY: 105,
        theme: 'grid',
        styles: {
            halign: 'center',
            fontSize: 10
        },
        headStyles: {
            fillColor: [51, 122, 183],
            textColor: [255, 255, 255]
        },
        bodyStyles: {
            fillColor: [245, 245, 245],
            lineColor: [200, 200, 200],
            lineWidth: 0.5
        },
        margin: { top: 5 },
    });

    // Total section
    const finalY = doc.lastAutoTable.finalY;
    doc.setFontSize(12);
    doc.text('Total', 150, finalY + 10);
    doc.setFontSize(14);
    doc.text(commande.prix_total + ' DT', 180, finalY + 10);
    
    // Texte supplémentaire
    var writtenNumber = require('written-number');
    let totalWords = writtenNumber(commande.prix_total, { lang: 'fr' }).toUpperCase();
    doc.setFontSize(11);
    doc.text(totalWords + ' DINARS', 100, finalY + 20, { align: 'center' });

    // Footer
    doc.setFontSize(10);
    doc.text('Nous apprécions votre clientèle', 110, doc.internal.pageSize.height - 30, 'center');
    doc.text('Si vous avez des questions sur cette facture, n\'hésitez pas à nous contacter.', 110, doc.internal.pageSize.height - 20, 'center');

    // Page number
    var pageCount = doc.internal.getNumberOfPages();
    for (let i = 0; i < pageCount; i++) {
        doc.setPage(i + 1);
        let pageCurrent = doc.internal.getCurrentPageInfo().pageNumber;
        doc.setFontSize(10);
        doc.text('Page: ' + pageCurrent + '/' + pageCount, 110, doc.internal.pageSize.height - 10, 'center');
    }

    // Enregistrement du PDF
    doc.save(`Cimenterie_${commande.code_commande}.pdf`);
}

  },
};
</script>

<style scoped>
.table {
  border-collapse: collapse;
  width: 100%;
  margin-bottom: 20px;
}

.table th, .table td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.table-striped tbody tr:nth-child(odd) {
  background-color: #f2f2f2;
}

.table-hover tbody tr:hover {
  background-color: #f1f1f1;
}

.btn {
  font-size: 0.9rem;
  padding: 6px 15px;
  border-radius: 5px;
  transition: background-color 0.3s, transform 0.2s;
}

.btn-danger {
  background-color: #e74c3c;
}

.btn-danger:hover {
  background-color: #c0392b;
  transform: scale(1.05);
}

.btn-success {
  background-color: #2ecc71;
}

.btn-success:hover {
  background-color: #27ae60;
  transform: scale(1.05);
}

.titre {
  font-size: 1.5rem;
  font-weight: 600;
  color: #333;
}
</style>
