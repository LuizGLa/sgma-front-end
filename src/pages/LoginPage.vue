<template>
  <q-page class="flex flex-center column" id="background-full">
    <div class="card-login">
      <q-img src="src/assets/Logo-SGMA.png" width="230px" height="200px"></q-img>

      <q-input filled type="email" v-model="email" :dense="dense" placeholder="Email" :rules="[emailRule]">
        <template v-slot:append>
          <q-icon name="email" />
        </template>
      </q-input>

      <q-input filled v-model="password" :dense="dense" type="password" placeholder="Senha">
        <template v-slot:append>
          <q-icon name="vpn_key" />
        </template>
      </q-input>

      <q-btn label="Fazer login" @click="login" type="send" color="primary" class="q-field button-login" />
    </div>
  </q-page>
</template>

<style scoped>
.card-login {
  max-width: 400px; /* Definindo uma largura máxima para o card */
  margin: 20px;
  padding: 20px;
  background: rgba(255, 255, 255, 0.842);
  border-radius: 10px;
  box-shadow: 0 4px 11px rgba(0, 0, 0, 0.651);
}

#background-full {
  background: linear-gradient(to right, #7ba6da, #0b3b94);
}

.button-login {
  width: 100%; /* Botão com largura igual aos inputs */
  margin-top: 20px;
}
</style>

<script>
import { defineComponent } from 'vue';
import { QIcon } from 'quasar';
import axios from 'axios';

export default defineComponent({
  name: 'LoginPage',
  components: {
    QIcon,
  },
  data() {
    return {
      email: '',
      password: '',
      dense: false,
      emailRule: [
        (v) => !!v || 'Email é obrigatório',
        (v) => /.+@.+/.test(v) || 'Email deve ser válido',
      ],
    };
  },
  methods: {
    async login() {
      try {
        const response = await axios.post('http://localhost:3000/user/login', {
          email: this.email,
          password: this.password,
        });

        if (response.data.success) {
          this.$router.push('/');
          console.log('Login bem-sucedido.');
        } else {
          this.email = '',
          this.password = ''
          console.error('Falha no login: Credenciais inválidas');
        }
      } catch (error) {
        console.error('Falha no login:', error);
      }
    },
  },
});
</script>
