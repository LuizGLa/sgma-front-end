<template>
    <q-page>
      <q-table
        :rows="alunos"
        :columns="columns"
        row-key="cpf"
      >
        <template v-slot:body-cell-actions="props" >
          <div class="actions flex flex-center">
           <q-btn
            icon="mdi-pencil"
            color="primary"
            @click="editAluno(props.row.id)"
            style="margin-right: 5px;"
          />
          <q-btn
            icon="mdi-delete"
            color="negative"
            @click="deleteAluno(props.row.id)"
          />
          
        </div>
        </template>
      </q-table>
    </q-page>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        alunos: [],
        columns: [
          {
            name: 'name',
            required: true,
            label: 'Nome',
            align: 'left',
            field: 'name',
            format: val => val,
            sortable: true
          },
          {
            name: 'cpf',
            required: true,
            label: 'CPF',
            align: 'left',
            field: 'cpf',
            format: val => val,
            sortable: true
          },
          {
            name: 'dateBirth',
            required: true,
            label: 'Data de Nascimento',
            align: 'left',
            field: 'dateBirth',
            format: (val) => {
              const date = new Date(val);
              return date.toLocaleDateString();
            },
            sortable: true
          },
          {
            name: 'actions',
            label: 'Ações',
            field: 'actions',
            align: 'center',
            sortable: false
          }
        ]
      };
    },
    created() {
      this.fetchAlunos();
    },
    methods: {
      async fetchAlunos() {
        try {
          const response = await axios.get('http://localhost:3000/alunos');
          this.alunos = response.data;
        } catch (error) {
          console.error('Erro ao buscar alunos:', error);
        }
      },
      async deleteAluno(id) {
        try {
          await axios.delete(`http://localhost:3000/alunos/${id}`);
          this.fetchAlunos();
        } catch (error) {
          console.error('Erro ao deletar aluno:', error);
        }
      },
      editAluno(id) {
        // Implementar a lógica para editar o aluno com o ID fornecido
      }
    }
  };
  </script>
  