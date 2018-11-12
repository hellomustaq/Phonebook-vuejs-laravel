<template>
<div>
    <nav class="panel column is-offset-2 is-8">
        <p class="panel-heading">
            VueJs Phonebook
            <button class="button is-link is-outlined" @click="openAdd">
                Add New
            </button>
            <span class="is-pulled-right" v-if="loading">
                <i class="fa fa-spinner fa-pulse fa-2x fa-fw"></i>
            </span>
            
            <!-- <span class="sr-only">Loading...</span> -->
        </p>
        <div class="panel-block">
            <p class="control has-icons-left">
            <input class="input is-small" type="text" placeholder="search" v-model="searchQuery">
            <span class="icon is-small is-left">
                <i class="fas fa-search" aria-hidden="true"></i>
            </span>
            </p>
        </div>

        <a class="panel-block is-active" v-for="item, key in temp">
            <span class="column is-9">
                {{item.name}}
            </span>
             <span class="panel-icon has-text-info column is-1">
                <i class="ion-edit" @click="openUpdate(key)"></i>
            </span>
            <span class="panel-icon has-text-danger column is-1">
                <i class="ion-android-delete" @click="del(key,item.id)"></i>
            </span>
            <span class="panel-icon has-text-primary column is-1" >
                <i class="ion-eye" @click="openShow(key)"></i>
            </span>
        </a>
    </nav>

    <Add :openmodal='addActive' @closeRequest="close"></Add>
    <Show :openmodal="showActive" @closeRequest="close"></Show>
    <Update :openmodal="updateActive" @closeRequest="close"></Update>

</div>
</template>

<script>
    let Add = require('./Add.vue');
    let Show = require('./Show.vue');
    let Update = require('./Update.vue');
    export default {
        components:{Add,Show,Update},
        data(){
            return{
                addActive:'',
                showActive:'',
                updateActive:'',
                lists:{},
                errors:{},
                loading:false,
                searchQuery:'',
                temp:''
            }
        },
        watch:{
            searchQuery(){
                if (this.searchQuery.length > 0) {
                    this.temp = this.lists.filter((item) =>{
                        return Object.keys(item).some((key)=>{
                            let string = String(item[key]);
                            return string.toLowerCase().indexOf(this.searchQuery.toLowerCase())>-1
                        });
                        
                    });
                }else{
                    this.temp=this.lists;
                }
            }
        },

        mounted(){
            axios.post('/getData').then((response) => this.lists=this.temp=response.data )
                .catch((error) => this.errors=error.response.data.errors)
        },
        
        methods:{
            openAdd(){
                this.addActive = 'is-active';
            },
            close(){
                this.addActive = this.showActive = this.updateActive= this.deleteActive= '';
            },
            openShow(key){
                this.$children[1].list=this.temp[key];
                this.showActive = 'is-active';
            },
            openUpdate(key){
                this.$children[2].list=this.temp[key];
                this.updateActive = 'is-active';
            },
            del(key,id){
                if (confirm("Are You Sure ?")) {
                    this.loading = !this.loading
                    axios.delete(`/phonebook/${id}`).then((response) => {this.lists.splice(key,1);this.loading=!this.loading})
                    .catch((error) => this.errors=error.response.data.errors)
                }
            }
        }
    }
</script>
