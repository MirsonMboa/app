<template>
  <q-page class="q-pa-lg">
    <h5 class="q-mt-none">Preencha os campos</h5>
    <form @submit.prevent="enviarAjuda">
      <q-input label="Nome" v-model="nome" required />
      <q-input label="Contato" v-model="contato" required />
      <q-input label="Email" v-model="email" type="email" required />
      <q-input label="Assunto" v-model="Assunto" required />
      <q-input
        label="Mensagem de Ajuda"
        v-model="mensagem"
        type="textarea"
        required
      />
      <q-btn
        color="primary"
        label="Enviar Ajuda"
        type="submit"
        href="https://wa.me/+258861174428?text=Mensagem%20de%20ajuda"
      >
        Enviar Ajuda
      </q-btn>
    </form>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "AjudaPage",
  data() {
    return {
      nome: "",
      contato: "",
      email: "",
      localizacao: "",
      mensagem: "",
    };
  },
  methods: {
    enviarAjuda() {
      if (
        this.nome &&
        this.contato &&
        this.email &&
        this.Assunto &&
        this.mensagem
      ) {
        // Obter o número do seu WhatsApp
        const whatsappNumber = "+258861174428";

        // Criar uma mensagem com os dados do formulário
        const message = `${this.mensagem}`;

        // Enviar a mensagem para o WhatsApp
        window.open(`https://wa.me/${+258861174428}?text=${message}`, "_blank");
      } else {
        alert("Por favor, preencha todos os campos obrigatórios.");
      }
    },
  },
});
</script>
