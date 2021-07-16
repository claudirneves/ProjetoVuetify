<template>
   <div>
    <NuxtLink to="/utensilio">Ir para Utensilio</NuxtLink>

     <v-row
    align="center"
    justify="space-around"
  >
    
    <v-btn
      depressed
      color="primary"
      v-on:click="utensilio()"
    >
      Utensilios
    </v-btn>
  </v-row>
       
    </div>
    
</template>

<script>

export default {
    layout: 'default',
    methods: {
      utensilio(){
        //this.$router.push('/utensilio');
        this.$toast.add({severity:'success',detail:'Utensílio Deletado com Sucesso!',life:5000});

        
    }
    }
    
}
</script>

