<template>
  <div>
    <article class="contact py-5" data-aos="fade-up">
      <div class="container py-5">
        <div class="text-center py-4">
          <h2 class="text-uppercase text-success">Contactez-nous</h2>
        </div>
        <form @submit.prevent="Onsubmit()" class="shadow p-4 m-3 rounded bg-light">
          <h4 class="fw-bolder text-success">Prenez contact</h4>
          <p class="text-muted">Nous sommes là pour vous ! Comment pouvons-nous vous aider ?</p>

          <div v-if="afficher_error" class="alert alert-danger">
            {{ error }}
          </div>

          <div class="row">
            <div class="col-lg-6">
              <div class="mb-3">
                <label for="name" class="form-label text-success">Votre nom</label>
                <input
                  type="text"
                  id="name"
                  v-model="name"
                  @keyup="validate('name')"
                  @click="validate('name')"
                  :class="name_error ? 'form-control is-invalid' : 'form-control'"
                  placeholder="Entrez votre nom"
                />
                <div v-if="name_error" class="invalid-feedback">
                  {{ name_error }}
                </div>
              </div>

              <div class="mb-3">
                <label for="email" class="form-label text-success">Votre email</label>
                <input
                  type="email"
                  id="email"
                  v-model="email"
                  @keyup="validate('email')"
                  @click="validate('email')"
                  :class="email_error ? 'form-control is-invalid' : 'form-control'"
                  placeholder="Entrez votre adresse email"
                />
                <div v-if="email_error" class="invalid-feedback">
                  {{ email_error }}
                </div>
              </div>

              <div class="mb-3">
                <label for="messages" class="form-label text-success">Votre message</label>
                <textarea
                  id="messages"
                  v-model="messages"
                  @keyup="validate('messages')"
                  @click="validate('messages')"
                  :class="messages_error ? 'form-control is-invalid' : 'form-control'"
                  rows="6"
                  placeholder="Allez-y, nous vous écoutons"
                ></textarea>
                <div v-if="messages_error" class="invalid-feedback">
                  {{ messages_error }}
                </div>
              </div>

              <button type="submit" class="btn btn-success w-100">Envoyer</button>
            </div>
          </div>
        </form>
      </div>
    </article>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      messages: "",
      name: "",
      email: "",
      name_error: "",
      email_error: "",
      messages_error: "",
      afficher_error: 0,
      error: "",
    };
  },
  methods: {
    async Onsubmit() {
      let valid =
        this.validate("name") && this.validate("email") && this.validate("messages");

      if (valid) {
        try {
          await axios.post("http://localhost:8000/api/contact", {
            name: this.name,
            email: this.email,
            message: this.messages,
          });

          // Success feedback
          this.$router.push("/success"); // Or show a success message
        } catch (err) {
          this.error = "Une erreur est survenue lors de l'envoi du message.";
          this.show_hide();
        }
      }
    },
    validate(input) {
      // Validation logic (same as before)
    },
  },
};
</script>
