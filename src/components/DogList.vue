<template>
    <div class="container p-3">
        <h1 class="text-center mb-4"> Razas de Perritos</h1>
        <div class="row">
            <div v-for="(item,index) in breeds" 
            :key="item.name"   
            class="col-md-4 mb-4"
            @click="goToDetail(item.name)"
            >

                <div class="card h-100  bg-gris">
                    <img :src="item.image" class="card-img-top" alt="imagen de raza de perro">
                    <div class="card-body  bg-gris">
                        <h4 class="card-title text-center text-white">{{ item.name }}</h4>

                    </div> 

                </div>



            </div>
        </div>

    </div>

</template>


<script>
import axios from 'axios'

export default {
    name: 'DogList',
    data(){
        return{
            breeds:[]
        }
    },
    methods:{
        async getBreeds(){

            try{
                const response = await axios.get('https://dog.ceo/api/breeds/list/all')
            
                const breedNames = Object.keys(response.data.message);
                console.log(breedNames);

                const breedData = await Promise.all(
                    breedNames.map( async (raza)=>{
                        const imgResponse= await axios.get(`https://dog.ceo/api/breed/${raza}/images/random`);
                        return {name: raza, image: imgResponse.data.message };
                    })
                );
                 this.breeds=breedData;


            }catch(e){
                console.log(e);
            }
        },
        goToDetail(breed){
            console.log('click');
           this.$router.push({name:'DogDetail', params:{breed} }); 

        },
    },
     //LIFECycles
     created(){
        this.getBreeds();
        console.log('se ejecuto el created');

     },
     mounted(){
     


     }
}


</script>


<style>
.card-img-top{
    object-fit: cover;
    height: 200px;
}
.bg-gris{
    background-color: rgb(133, 131, 131);
}


</style>
