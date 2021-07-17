<template>

 <v-form
    ref="form"
    v-model="valida"
    lazy-validation
    @submit="save()"
  >
    <v-text-field
      v-model="utensilio.descricao"
      :counter="20"
      :rules="descricaoRules"
      label="Descrição"
      required
    ></v-text-field>


    <v-select
      v-model="utensilio.disponivel"
      :items="items"
      :rules="[v => !!v || 'Seleção é Obrigatória']"
      label="Disponível"
      required
    ></v-select>

    <v-btn
      :disabled="!valida"
      color="success"
      class="mr-4"
      @click="save()"
    >
      Salvar
    </v-btn>
    <v-dialog
      width="500"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="red"
          dark
          v-bind="attrs"
          v-on="on"
        >
          Listar
        </v-btn>
      </template>

      <v-card>
        <v-card-title class="text-h5 grey lighten-2">
          Lista de Utensílios
        </v-card-title>

        <v-card-text>
          <v-data-table
            :headers="headers"
            :items="utensilios"
            :items-per-page="5"
            class="elevation-1"
            :footer-props="{
            showFirstLastPage: true,
            pageText: '{0}-{1} de {2}' ,
            'items-per-page-text':'Linhas por Página'
      }"
      
         >
         <template v-slot:[`item.actions`]="{ item }">
           <v-btn
              @click="edit(item)"
              color="warning"
              title="Editar registro"
              icon
              large
            >
              <v-icon color="warning">mdi-pencil</v-icon>
            </v-btn>
           <v-btn
              @click="del(item)"
              color="error"
              title="Deletar registro"
              icon
              large
            >
              <v-icon color="error">mdi-delete</v-icon>
            </v-btn>
         </template>
        </v-data-table>
 
        </v-card-text>

        <v-divider></v-divider>

        
      </v-card>
    </v-dialog>
  </v-form>
</template>

<script>
import UtensilioService from '~/service/UtensilioService'
  export default {
    data: () => ({
      
      valida: true,
      descricaoRules: [
        v => !!v || 'Descrição Obrigatória',
        v => (v && v.length >= 3) || 'Descrição deve ter mais que 3 caracteres',
      ],
      
      select: null,
      items: [
        'SIM',
        'NÃO',
      ],
      
    }),
    layout:'default',
    async asyncData({params, error}){
      return {
        headers: [
          {
            text: 'Descrição',
            align: 'start',
            value: 'descricao',
          },
          {
            text:'Disponível',
            align: 'start',
            value: 'disponivel',
          },
          {
            text: 'Ações',
            align: 'center',
            value: 'actions'
          }
          ],
        utensilios: [],
        id: params.id,
        utensilio: {
          id: null,
          descricao: '',
          disponivel: ''
        }
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
      },
      async save() {
        let values = this.utensilio;
        this.utensilioService.save(values).then((response)=> {this.$toast.add({severity:'success',detail:'Utensilio salvo com sucesso',life:5000});
        this.utensilioList();
        this.$router.push('/utensilio');
        })
        .catch((error)=> {
          this.$toast.add({severity:'error',detail:'Não foi possível realizar o cadastro',life:5000});
        });
      },
      edit(row){
        this.$router.push('/utensilio/'+ row.id);
        this.utensilioList();
      },
      
     async del(row){
        var retorno;
        retorno = confirm('Clique OK para excluir');
        if(retorno==true){
          try{
            let response = await this.utensilioService.delete(row.id);
            this.utensilioList();
            console.log('passou aqui'+ row.id);
            
          }catch(err){
            console.log('passou aqui com erro: '+row.id);
            
          }
        }
      }
      
    }
  }
</script>