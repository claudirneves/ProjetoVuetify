<template>
  <div id="app">
  <v-app id="inspire">
    <v-card>
      <v-card-title>
        Lista de Utensílios
        <v-spacer></v-spacer>
        <v-text-field
          label="Busca"
          single-line
          hide-details
          v-model="search"
        ></v-text-field>
      </v-card-title>
      <v-data-table
         v-model="selected"
        :headers="headers"
        :items="utensilios"
	      :items-per-page="5"
        :search="search" 
	      class="elevation-1"
                    
      >
      
        <template slot="utensilios" slot-scope="props">
          <tr @click="showAlert(props.item)">
          <td>{{ props.item.descricao }}</td>
          <td class="text-xs-right">{{ props.item.disponivel }}</td>
            </tr>
        </template>
        <v-alert slot="no-results" :value="true" color="error" icon="warning">
          Sua pesquisa por "{{search}}" não encontrou resultados.
        </v-alert>
      </v-data-table>
    </v-card>
  </v-app>
</div>
</template>

<script>
import UtensilioService from '~/service/UtensilioService'
  export default {
    layout: 'default',
     methods: {
    showAlert(a){
      if (event.target.classList.contains('btn__content')) return;
      alert('Alert! \n' + a.descricao);
    }
  },
   data ({params, error}) {
      return {
        selected:[],
        search: '',
        headers: [
          {
            text: 'Descrição',
            align: 'start',
            sortable: false,
            value: 'descricao',
          },
          {
            text:'Disponível',
            align: 'start',
            sortable: false,
            value: 'disponivel',
          }
          ],
        utensilios: []
      }
    },
    utensilioService: null,
    created(){
      this.utensilioService = new UtensilioService(this.$axios);
    },
    mounted(){
      this.utensilioList();
    },
    methods: {
      async utensilioList(){
        let response = await this.utensilioService.list();
        this.utensilios = response.content;
      }
    }
  }
</script>