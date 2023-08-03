<template>
  <q-page class="flex flex-center">
    
    <q-card
      class="my-card text-white"
      style="background: radial-gradient(circle, #1d6eb4 0%, #012c52 100%)"
    >
      <q-card-section class="row">
        <q-icon name="person_outline" size="md" style="margin-right: 5px;"/>
        <div class="text-h5">Alunos Cadastrados:</div>
        
      </q-card-section>

      <q-card-section class="q-pt-none">
        
      </q-card-section>
    </q-card>

  </q-page>
</template>

<style scoped>
 .q-page {
  background: linear-gradient( to top, rgb(255, 255, 255), rgba(121, 143, 185, 0.527));
 }
</style>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'IndexPage'
})
</script>
