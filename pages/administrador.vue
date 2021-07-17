<template>
   <div>
    <NuxtLink to="/utensilio">Ir para Utensilio</NuxtLink>      
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

