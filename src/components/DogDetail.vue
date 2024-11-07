<template>
    <div class="container p-3">
        <h1 class="text-center mb-4"> Imagenes de: {{ breed }} </h1>
        <button @click ="goToBack" class=" btn btn-secondary mb-4">← Volver </button>

        <div v-if="images.length" class="row">
         
            <div v-for="(item,index) in images" 
            :key="index"   
            class="col-md-4 mb-4"
            @click="goToDetail(item.name)"
            >

            <img :src="item" class="card-img-top" alt="imagen de raza de perro">
                
        </div>

    </div>
</div>
</template>


<script>
import axios from 'axios'

export default {
    name: 'DogDetail',
    props:['breed'],
    data(){
        return{
            images:[]
        }
    },
    methods:{
        async getImages(){

             try{
                const response = await axios.get(`https://dog.ceo/api/breed/${this.breed}/images`)
                this.images= response.data.message;

            }catch(e){
                console.log(e);
            } 
        },
        goToBack(){
            this.$router.push('/')
        }
    },
     //LIFECycles
    created(){
        this.getImages();
    },


     mounted(){
      

     }
}


</script>


<style>
</style>
