<template>
  <q-page class="flex flex-center column">
    
   <div class="container">
    <q-card
      class="my-card text-white"
      style="background: radial-gradient(circle, #1d6eb4 0%, #012c52 100%)"
    
    >
      <q-card-section class="row">
        <q-icon name="person_outline" size="md" style="margin-right: 10px;"/>


        <div 
         class="text-h5"
         style=" font-family: Lexend;"> 
         Alunos Cadastrados: 
        </div>
         
      </q-card-section>
      <div 
      class="text-h2 flex flex-center"
      style=" font-family: sans-serif;">
      {{ totalAlunos }} 
      </div>
    </q-card>
    <q-btn label="Gerenciar alunos" to="/alunos" type="send" color="primary" class="q-field button-alunos" />
  </div> 
  </q-page>
</template>

<style scoped>
 @import url('https://fonts.googleapis.com/css2?family=Lexend:wght@900&display=swap');
 .q-page {
 background: linear-gradient( to top, rgb(255, 255, 255), rgba(121, 143, 185, 0.527));
 }

 .button-alunos {
  width: 100%; 
  margin-top: 5px;

}

</style>
  
<script>

import axios from 'axios';
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'IndexPage',
   

   data() {
      return {
      totalAlunos: null,
    };
   },
   async mounted() {
    try {
      const response = await axios.get('http://localhost:3000/alunos/count'); // Certifique-se de que a rota esteja correta
      this.totalAlunos = response.data.total;
    } catch (error) {
      console.error('Erro ao obter o total de alunos:', error);
    }
  },
    })
</script>
