<template>
    <div>

        <nav class="navbar navbar-light bg-light justify-content-between">
            <a class="navbar-brand">Urban Dictionary</a>
            <form class="form-inline" v-on:submit.prevent="onSubmit">
                <input class="form-control mr-sm-2" type="search" placeholder="Search" aria-label="Search" v-model="text" v-on:keyup.enter="getDefinitions">
                <button class="btn btn-outline-success my-2 my-sm-0" @click="getDefinitions">Search</button>
            </form>
        </nav>
        
        <div>
            <div><h3> {{ searchText }}</h3></div><br>
            
            <div v-for="definition in definitions" v-bind:key="definition.defid">
                <h5>Definition</h5>
                <p>{{definition.definition}}</p>                
                <p><strong>Example: </strong> {{ definition.example }}</p>
                <small><em>Author: {{ definition.author}}</em></small>
                <hr>
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
            searchText: ""
        }
    },
    methods: {
        getDefinitions: function() {
            this.searchText = this.text
            axios.get('http://api.urbandictionary.com/v0/define?term=' + this.text)
            .then(response => {
                console.log(response.data.list)
                this.definitions = response.data.list
            })            
            .catch(err => console.log(err))
        }
    }
}
</script>

<style>

</style>
