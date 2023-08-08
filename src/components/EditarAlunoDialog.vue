<template>
    <q-dialog v-model="dialogVisible" @hide="resetState">
      <q-card>
        <q-card-section>
          <q-input v-model="alunoEditado.name" label="Nome" />
          <q-input v-model="alunoEditado.cpf" label="CPF" />
          <q-input v-model="alunoEditado.dateBirth" label="Data de Nascimento" />
        </q-card-section>
        <q-card-actions align="right">
          <q-btn label="Cancelar" color="negative" @click="closeDialog" />
          <q-btn label="Atualizar" color="primary" @click="atualizarAluno" />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </template>
  
  <script>
import axios from 'axios';
  export default {
    props: ['aluno'],
    data() {
      return {
        dialogVisible: true,
        alunoEditado: { ...this.aluno }, // Faça uma cópia do aluno para edição
      };
    },
    methods: {
    closeDialog() {
      this.dialogVisible = false;
      this.$emit('hide');
    },
    resetState() {
      this.dialogVisible = false;
    },
    async atualizarAluno() {
      try {
        await axios.patch(`http://localhost:3000/alunos/${this.aluno.id}`, this.alunoEditado);
        this.$emit('atualizarAluno', this.alunoEditado);
        this.closeDialog();
      } catch (error) {
        console.error('Erro ao atualizar aluno:', error);
      }
    },
  },
};
  </script>
  