<template>
    <div>

        <nav class="navbar navbar-light bg-dark indigo justify-content-between fixed">
            <h6 class="navbar-brand text-white">Urban Dictionary</h6>
        </nav>

        <div class="container">
            <div class="input-group">
                <input type="text" class="form-control" placeholder="Search" v-model="text" v-on:keyup.enter="getDefinitions">
                <div class="input-group-append">
                    <button class="btn btn-secondary" type="button" @click="getDefinitions">
                        <i class="fa fa-search"></i>
                    </button>
                </div>
            </div>
            <br>
            <div class="text-right" v-if="fav">
                <a class="btn-floating btn-sm btn-secondary"><span class="text-white mr-1">{{ favorite }} </span> <i class="fa fa-heart fa-lg text-white"></i></a>
            </div>

            <div class="definitions">
                
                
                <div v-for="definition in definitions" v-bind:key="definition.defid">
                    <div class="card">
                        <div class="card-body">
                            <h3 class="card-title word"> {{ searchText }}</h3>
                            <p class="card-text">{{definition.definition}}</p>                
                            <p><em> {{ definition.example }}</em></p>
                            <small>By: <cite title="Source Title">{{ definition.author}}</cite></small>
                            <div class="float-right">
                                <a class="card-link"><i class="fa fa-thumbs-up"> {{ definition.thumbs_up }}</i></a> 
                                <a class="card-link"><i class="fa fa-thumbs-down"> {{ definition.thumbs_down }}</i></a>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="" v-if="beforeResult">
                <div class="text-center"><i class="fa fa-5x fa-arrow-up cyan-text"></i></div>
                <h3 class="text-center">Search for a word using the search box above</h3>
            </div>

            <div class="" v-if="noResult">
                <div class="text-center"><i class="fas fa-sad-tear"></i></div>
                <h3>Search word not found</h3>
                <p>Try another word</p>
            </div>

        </div>

        <footer class="page-footer font-small fixed-bottom footer">

           
            <div class="container-fluid text-center footer-link">
                <div class="container">
                
                    <div class="row">

                    
                        <div class="col-4 text-center mt-3 mb-3">
                            <button class="btn btn-flat" id="modalActivate" data-toggle="modal" data-target="#exampleModalPreview" @click="historyResult"><i class="fa fa-history fa-lg text-white"></i><span class="badge badge-info ml-2">{{ history.length }}</span>
                            </button>
                        </div>
                        

                        

                        
                        <div class="col-4 text-center mt-3 mb-3">
                            <button class="btn btn-flat"><i class="fa fa-heart fa-lg text-white"></i><span class="badge badge-info ml-2">9</span>
                            </button>
                        </div>
                        

                        

                        
                        <div class="col-4 text-center mt-3 mb-3">
                            <button class="btn btn-flat"><i class="fa fa-info-circle fa-lg text-white"></i></button>
                        </div>
                        

                    </div>
                    
                </div>
            </div>
           

            <div class="footer-copyright text-center py-3">© 2019 Copyright:
                <a href="#">D Weird Coder</a>
            </div>
            
        </footer>

        <!-- Button trigger modal -->
        <!-- <button id="modalActivate" type="button" class="btn btn-danger" data-toggle="modal" data-target="#exampleModalPreview">
        Launch demo modal
        </button> -->
        <!-- Modal -->
        <div class="modal fade right" id="exampleModalPreview" tabindex="-1" role="dialog" aria-labelledby="exampleModalPreviewLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalPreviewLabel">History</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true" class="white">&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                    <ul class="list-group list-group-flush">
                        <li class="list-group-item" v-for="history in history" :key="history"><h6>{{ history }}</h6></li>
                    </ul>
                    <h5 v-if="noHistory">No search have been made yet.</h5>
                </div>
                <!-- <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                    <button type="button" class="btn btn-primary">Save changes</button>
                </div> -->
            </div>
        </div>
        </div>
        <!-- Modal -->

    </div>
</template>

<script>

import axios from 'axios'

export default {
    name: "Search",
    data() {
        return {
            text: "",
            definitions: [],
            searchText: "",
            beforeResult: true,
            noResult: false,
            favorite: "Like",
            fav: false,
            history: [],
            noHistory: false
        }
    },
    methods: {
        getDefinitions: function() {
            this.definitions = []
            this.history.push(this.text)
            var self = this
            this.searchText = this.text
            axios.get('http://api.urbandictionary.com/v0/define?term=' + this.text)
            .then(function(response) {
                // console.log(response.data.list)
                if(response.data.list.length === 0) {
                    self.noResult = true
                    self.fav = false
                    // self.history.push(self.text)
                } else {
                self.definitions = response.data.list
                self.fav = true
                }
            })            
            .catch(function (error) {
                console.log(error);
            })

            this.beforeResult = false
            console.log(this.history)
        },
        historyResult: function() {
            console.log(this.history)
            if (this.history.length === 0) {
                this.noHistory = true
            }else{
                this.noHistory = false
            }
        }
    }
}
</script>

<style scoped>
    .word {
        text-transform: capitalize;
        color: blue;
    }

    nav {
        margin-bottom: 1rem;
    }

    .card {
        margin-top: 1rem;
        margin-bottom: 1rem;
    }
    .definitions {
        margin-bottom: 9rem;
    }
    .footer {
        background: #343a40;
        color: #fff;
    }
    .footer-link {
        background: #343A5F;
        color: #fff;
    }

    .modal-dialog {
        /* top: 0px !important; */
        margin-top: 0px;
        /* margin-bottom:0px; */
        right: 0;
        position: absolute !important;
        width: 50vw;
    }
    .modal-content {
        /* right: 0px; */
        height: 100vh;
    }
    .modal-header {
        background: #343A5F;
        color: #fff;
    }
    .white {
        color: #fff;
    }
</style>
