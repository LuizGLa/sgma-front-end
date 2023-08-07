<template>
  <q-page >
    <div 
    class="text-h4 flex flex-center"
    style=" font-family: Lexend; color: #0a2e52">
      Alunos Cadastrados
    </div>

    <q-table
      :rows="alunos"
      :columns="columns"
      row-key="cpf"
      class="my-sticky-header-table q-table__middle"
    >
      <template v-slot:body-cell-actions="props">
        <div class="actions flex flex-center" style="margin-top: 10px;">
          <q-btn
            icon="edit"
            color="primary"
            @click="editAluno(props.row.id)"
            style="margin-right: 5px;"
            size="sm"
          />
          <q-btn
            icon="delete"
            color="negative"
            @click="deleteAluno(props.row.id)"
            size="sm"
          />
          </div>
        
      </template>
    </q-table>
  </q-page>
</template>

<style lang="sass">

.my-sticky-header-table
  /* height or max-height is important */
  height: 310px

  .q-table__top,
  .q-table__bottom,
  thead tr:first-child th
    /* bg color is important for th; just specify one */
    background-color: #0a2e52
    color: white

  thead tr th
    position: sticky
    z-index: 1
  thead tr:first-child th
    top: 0

  /* this is when the loading indicator appears */
  &.q-table--loading thead tr:last-child th
    /* height of all previous header rows */
    top: 48px

  /* prevent scrolling behind sticky top row on focus */
  tbody
    /* height of all previous header rows */
    scroll-margin-top: 48px

    
</style>


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
  