<template>
    <div>
        <nav class="panel column is-offset-2 is-8">
            <p class="panel-heading">
                Vue JS Phonebook
                <button class="button is-primary is-outlined" @click="openAdd">
                    Add New
                </button>
                <span class="is-pulled-right" v-if="loading">
                    <i class="fa fa-spinner fa-spin" style="font-size:24px"></i>
                </span>
            </p>
            <div class="panel-block">
                <p class="control has-icons-left">
                    <input class="input is-small" type="text" placeholder="search" v-model="searchQuery">
                    <span class="icon is-small is-left">
                    <i class="fas fa-search" aria-hidden="true"></i>
                </span>
                </p>
            </div>
            <a class="panel-block" v-for="(item, key) in temp ">
            <span class="column is-9">
                {{item.name}}
            </span>
                <span class="panel-icon column is-1">
             <i class="has-text-danger fa fa-trash" aria-hidden="true" @click="del(key, item.id)"></i>
            </span>
                <span class="panel-icon column is-1">
              <i class="has-text-info fa fa-edit" aria-hidden="true" @click="openUpdate(key)"></i>
            </span>
                <span class="panel-icon column is-1">
              <i class="has-text-primary fa fa-eye" aria-hidden="true" @click="openShow(key)"></i>
            </span>
            </a>
        </nav>
        <Add :openmodal="addActive" @closeRequest="close"></Add>
        <Show :openmodal="showActive" @closeRequest="close"></Show>
        <Update :openmodal="updateActive" @closeRequest="close"></Update>
    </div>
</template>

<script>
    let Add = require('./Add');
    let Show = require('./Show');
    let Update = require('./Update');
    export default {
        name: "home",
        data() {
            return {
                addActive: '',
                showActive: '',
                updateActive: '',
                lists: {},
                errors: {},
                loading: false,
                searchQuery: '',
                temp: ''
            }
        },
        watch: {
            //displaying every result matches name/phone/email
            searchQuery() {
                if (this.searchQuery.length > 0) {
                    this.temp = this.lists.filter((item) => {
                        return Object.keys(item).some((key) => {
                            return String(item[key]).toLowerCase()
                                .indexOf(this.searchQuery.toLowerCase()) > -1;
                        })

                    });
                } else {
                    //to display populated result after search
                    this.temp = this.lists
                }
            }
        },

        mounted: function () {
            axios.post('http://localhost:8888/MY/phonebook_lar_vue/public/getData')
                .then((response) => this.lists = this.temp = response.data)
                .catch((error) => this.errors = error.response.data.errors);
        },
        components: {Add, Show, Update},
        methods: {
            openAdd() {
                this.addActive = 'is-active';
            },
            openShow(key) {
                this.$children[1].list = this.temp[key];
                this.showActive = 'is-active';
            },
            openUpdate(key) {
                this.$children[2].list = this.temp[key];
                this.updateActive = 'is-active';
            },
            close() {
                this.addActive = '';
                this.showActive = '';
                this.updateActive = '';
            },
            del(key, id) {
                if (confirm('Are you sure?')) {
                    this.loading = !this.loading;
                    axios.delete(`http://localhost:8888/MY/phonebook_lar_vue/public/phonebook/${id}`)
                        .then((response) => {
                            this.lists.splice(key, 1);
                            this.loading = !this.loading;
                        })
                        .catch((error) => this.errors = error.response.data.errors);
                }
                console.log(`${key} ${id}`);
            }
        }
    }
</script>

<style scoped>

</style>