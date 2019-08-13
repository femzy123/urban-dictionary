<template>
    <div>

        <nav class="navbar navbar-light bg-dark indigo justify-content-between fixed">
            <h6 class="navbar-brand text-white">Urban Dictionary</h6>
        </nav>

        <div class="alert alert-warning" role="alert" v-if="warning">
            Oops!! There was an error ({{ warningText }}). Please <a href="/" class="alert-link">try again</a>.
        </div>

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
                <a class="btn-floating btn-sm btn-secondary" @click="liked" :style="{backgroundColor: likeColor}"><span class="text-white mr-1">{{ likeText }} </span> <i class="fa fa-heart fa-lg text-white"></i></a>
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
                            <button class="btn btn-flat" id="modalActivate" data-toggle="modal" data-target="#favoriteModal" @click="favoriteResult"><i class="fa fa-heart fa-lg text-white"></i><span class="badge badge-info ml-2">{{ favorite.length }}</span>
                            </button>
                        </div>
                        

                        

                        
                        <div class="col-4 text-center mt-3 mb-3">
                            <button class="btn btn-flat" id="modalActivate" data-toggle="modal" data-target="#aboutModal"><i class="fa fa-info-circle fa-lg text-white"></i></button>
                        </div>
                        

                    </div>
                    
                </div>
            </div>
           

            <div class="footer-copyright text-center py-3">© 2019 Copyright:
                <a href="https://dweirdcoder.com" target="blank">D Weird Coder</a>
            </div>
            
        </footer>

        <!-- Button trigger modal -->
        <!-- <button id="modalActivate" type="button" class="btn btn-danger" data-toggle="modal" data-target="#exampleModalPreview">
        Launch demo modal
        </button> -->
        <!-- Modal -->
        <div class="modal fade right" id="exampleModalPreview" tabindex="-1" role="dialog" aria-labelledby="exampleModalPreviewLabel" aria-hidden="true">
        <div class="modal-dialog modal-dialogStyle" role="document">
            <div class="modal-content modal-contentStyle modal-dialog-scrollable">
                <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalPreviewLabel"><i class="fa fa-history fa-lg text-white"></i> History</h5>
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

        <div class="modal fade right" id="favoriteModal" tabindex="-1" role="dialog" aria-labelledby="favoriteModal" aria-hidden="true">
            <div class="modal-dialog modal-dialogStyle modal-dialog-scrollable" role="document">
                <div class="modal-content modal-contentStyle">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalPreviewLabel"><i class="fa fa-heart fa-lg text-white"></i> Favorites</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true" class="white">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        <ul class="list-group list-group-flush">
                            <li class="list-group-item" v-for="favorite in favorite" :key="favorite"><h6>{{ favorite }}</h6></li>
                        </ul>
                        <h5 v-if="noFavorite">You have no favorite words.</h5>
                    </div>
                    <!-- <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                        <button type="button" class="btn btn-primary">Save changes</button>
                    </div> -->
                </div>
            </div>
        </div>

        <div class="modal fade right" id="aboutModal" tabindex="-1" role="dialog" aria-labelledby="aboutModal" aria-hidden="true">
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalPreviewLabel">About</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true" class="white">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        <div class="mb-3 mt-3">
                            <img class="img img-thumbnail rounded mx-auto d-block" src="images/urban_dictionary.jpg">
                            <h4 class="text-center">Urban Dictionary</h4>
                            <p class="text-center">A simple urban dictionary built by <a href="https://dweirdcoder.com" target="blank">D Weird Coder</a></p>
                        </div>
                    </div>
                    <!-- <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                        <button type="button" class="btn btn-primary">Save changes</button>
                    </div> -->
                </div>
            </div>
        </div>

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
            fav: false,
            history: [],
            noHistory: false,
            like: false,
            likeText: "Like",
            likeColor: "#343A5F",
            favorite: [],
            noFavorite: false,
            warning: false,
            warningText: ""
        }
    },
    methods: {
        getDefinitions: function() {
            this.definitions = []
            this.history.push(this.text)
            var self = this
            this.searchText = this.text
            axios.get('https://api.urbandictionary.com/v0/define?term=' + this.text)
            .then(function(response) {
                if(response.data.list.length === 0) {
                    self.noResult = true
                    self.fav = false
                    // self.history.push(self.text)
                } else {
                self.definitions = response.data.list
                self.fav = true
                self.likeText = "Like"
                self.likeColor = "#343A5F"
                self.like = false
                }
            })            
            .catch(function (error) {
                self.warningText = error
                self.warning = true
            })

            this.beforeResult = false
        },
        historyResult: function() {
            if (this.history.length === 0) {
                this.noHistory = true
            }else{
                this.noHistory = false
            }
        },
        liked: function() {
            this.like = !this.like
            if (this.like === true) {
                this.likeText = "Liked"
                this.likeColor = "red"
                this.favorite.push(this.text)
            } else {
                this.likeText = "Like"
                this.likeColor = "#343A5F"
                this.favorite.pop(this.text)
            }
        },
        favoriteResult: function() {
            if (this.favorite.length === 0) {
                this.noFavorite = true
            }else{
                this.noFavorite = false
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

    .modal-dialogStyle {
        /* top: 0px !important; */
        margin-top: 0px;
        /* margin-bottom:0px; */
        right: 0;
        position: absolute !important;
        width: 50vw;
    }
    .modal-contentStyle {
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
    .red {
        background-color: red;
    }
    .blue {
        background: #343A5F;
    }
    .img {
        width: 50%
    }
</style>
