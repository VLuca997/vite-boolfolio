<script>
import axios from 'axios';


// my-component.js
export default {
    
    data() {
    return {
        projects: [],
        currentPage: 1,
        lastPage: 1,
        loading:false

    }
    },
    created(){
        this.getProject();
        this.changePage();
        
    },
    methods: {
        getProject(){
            if(!this.loading){
                this.loading = true;
                axios
                    .get('http://localhost:8000/api/projects',{
                        params:{page : this.currentPage}
                    })
                    .then(res =>{
                        this.projects = res.data.results.data;
                        this.currentPage = res.data.results.current_page;
                        this.lastPage = res.data.results.last_page;
                        this.loading = false;

                });
            }
        },
        changePage(mode){
            if(!this.loading){
                switch(mode){
                    case "+":
                        if (this.currentPage < this.lastPage) {
                            this.currentPage++;
                            this.getProject();
                        };
                    break;
                    case "-":
                    if (this.currentPage > 1) {
                            this.currentPage--;
                            this.getProject();
                        };
                    break;
                }
            }
        },
       
        
    },
}
</script>
<template>
    <div>
        
        <div class="container">
            <div class="row">
                <div class="col" v-for="project in projects" :key="project.id">
                    <h3 class="text-light bg-success p-2">
                        {{ project.title }}
                    </h3>
                </div>
                <div class="p-3">
                    <button class="btn btn-primary p-2 m-2" @click="changePage('-')">SINISTRA</button>
                    <button class="btn btn-primary p-2 m-2" @click="changePage('+')" > DESTRA</button>
                </div>
            </div>
        </div>
            
    </div>
</template>
<style lang="scss" scoped>



</style>