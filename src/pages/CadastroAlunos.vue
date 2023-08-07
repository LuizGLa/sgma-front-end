<template>
  <q-page >
    <div 
    class="text-h4 flex flex-center"
    style=" font-family: Lexend; color: #0a2e52">
      Alunos Cadastrados
    </div>
   <div class="areaTable flex flex-center">
    <q-table
      :rows="alunos"
      :columns="columns"
      row-key="cpf"
      class="my-sticky-header-table q-table__middle"
    >
      <template v-slot:body-cell-actions="props">
        <div class="actions flex flex-center" style="margin-top: 10px; ">
          <q-btn
            icon="update"
            color="warning"
            @click="editAluno(props.row.id)"
            style="margin-right: 5px;"
            size="sm"
            > 
            <q-tooltip 
            v-if="showTooltip">
            {{ attAl }}
            </q-tooltip>
           </q-btn>
          <q-btn
            icon="add"
            color="primary"
            @click="addModulo(props.row.id)"
            style="margin-right: 5px;"
            size="sm"
            > 
            <q-tooltip 
            v-if="showTooltip">
            {{ addMd }}
            </q-tooltip>
           </q-btn>
          <q-btn
            icon="delete"
            color="negative"
            @click="deleteAluno(props.row.id)"
            size="sm"
          >
          <q-tooltip 
            v-if="showTooltip">
            {{ delAl }}
           </q-tooltip>
          </q-btn>
          </div>
        
      </template>
    </q-table>
  </div>
    <div class="cadastro flex flex-center" style="margin-top: 10px;">
      <q-btn icon="add" label="Novo aluno" color="primary" @click="toggleCadastro" />
    </div>

    <div class="conatiner flex flex-center">
    <div v-if="cadastroVisible" style="width: 600px; height: 250px; margin-top: 20px; box-shadow: 0px 0px 10px rgba(0, 1, 5, 0.356);">
      <q-card class="q-mb-md">
        <q-card-section >
          <q-input v-model="nome" label="Nome" />
          <q-input v-model="cpf" label="CPF" mask="###.###.###-##"/>
          <q-input v-model="dataNascimento" label="Data de Nascimento" type="date" mask="####-##-##" />
        </q-card-section>
        <q-card-actions align="right">
          <q-btn label="Cancelar" color="negative" @click="cancelCadastro" />
          <q-btn label="Cadastrar" color="primary" @click="cadastrarAluno" />
        </q-card-actions>
      </q-card>
    </div>
  </div>
  </q-page>
</template>

<style lang="sass">

.my-sticky-header-table
  /* height or max-height is important */
  height: 400px
  width: 1800px

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
        cadastroVisible: false,
        nome: '',
        cpf: '',
        dataNascimento: '',
        showTooltip: true,
        addMd: "Adicionar módulo",
        delAl: "Remover aluno",
        attAl: "Atualizar aluno",
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
      toggleCadastro() {
      this.cadastroVisible = !this.cadastroVisible;
      this.clearCadastroFields();
    },
    clearCadastroFields() {
      this.nome = '';
      this.cpf = '';
      this.dataNascimento = '';
    },
    cancelCadastro() {
      this.cadastroVisible = false;
      this.clearCadastroFields();
    },
    async cadastrarAluno() {
      const novoAluno = {
        name: this.nome,
        cpf: this.cpf,
        dateBirth: this.dataNascimento,
      };
      try {
        await axios.post('http://localhost:3000/alunos', novoAluno);
        this.alunos.push(novoAluno);
        this.cancelCadastro();
      } catch (error) {
        console.error('Erro ao cadastrar aluno:', error);
      }
    },
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

      },

      addModulo(id) {
        // Implementar a lógica para modulo ao aluno com o ID fornecido
      },
      
      openCadastroDialog() {
      this.cadastroDialogVisible = true;
      console.log("teste")
    },
    alunoCadastrado(novoAluno) {
      this.alunos.push(novoAluno);
      console.log("teste")
    }
    }
  };
  </script>
  