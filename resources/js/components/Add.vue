<template>
    <div class="modal" :class="openmodal">
        <div class="modal-background"></div>
        <div class="modal-card">
            <header class="modal-card-head">
                <p class="modal-card-title">Add new entry</p>
                <button class="delete" aria-label="close" @click='closeAdd'></button>
            </header>
            <section class="modal-card-body">
                <div class="field">
                    <label class="label">Name</label>
                    <div class="control has-icons-left has-icons-right">
                        <input v-model="list.name" :class="{'is-danger':errors.name}" class="input is-success" type="text" placeholder="Name" name="name">
                        <span class="icon is-small is-left">
                            <i class="fa fa-user"></i>
                        </span>
                        <span class="icon is-small is-right">
                            <i class="fas fa-check"></i>
                        </span>
                    </div>
                    <p v-if="errors.name" class="help is-danger">{{errors.name[0]}}</p>
                </div>
                <div class="field">
                    <label class="label">Phone</label>
                    <div class="control has-icons-left has-icons-right">
                        <input v-model="list.phone" :class="{'is-danger':errors.phone}" class="input is-success" type="number" name="phone" placeholder="Phone" >
                        <span class="icon is-small is-left">
                            <i class="fa fa-user"></i>
                        </span>
                        <span class="icon is-small is-right">
                            <i class="fas fa-check"></i>
                        </span>
                    </div>
                    <p v-if="errors.phone" class="help is-danger">{{errors.phone[0]}}</p>
                </div>
                <div class="field">
                    <label class="label">Email</label>
                    <div class="control has-icons-left has-icons-right">
                        <input v-model="list.email" :class="{'is-danger':errors.email}" class="input is-success" type="email" placeholder="Email" name="email">
                        <span class="icon is-small is-left">
                            <i class="fa fa-user"></i>
                        </span>
                        <span class="icon is-small is-right">
                            <i class="fas fa-check"></i>
                        </span>
                    </div>
                    <p v-if="errors.email" class="help is-danger">{{errors.email[0]}}</p>
                </div>
            </section>
            <footer class="modal-card-foot">
                <button class="button is-success" @click="save">Save</button>
                <button class="button" @click='closeAdd'>Cancel</button>
            </footer>
        </div>
    </div>
</template>
<script>
    export default {
        props: ['openmodal'],
        data(){
            return{
                list:{
                    name:'',
                    phone:'',
                    email:'',
                },
                errors:{},
            }
        },
        methods: {
            closeAdd: function () {
                this.$emit('closeRequest');
            },
            save : function(){
                axios.post('/phonebook',this.$data.list).then((response) => {
                    this.closeAdd()
                    this.$parent.lists.push(response.data)
                    })
                .catch((error) => this.errors=error.response.data.errors)
            }
            
        }
    }
</script>
