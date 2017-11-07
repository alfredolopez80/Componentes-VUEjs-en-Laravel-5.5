<template>
    <div id="app">
        <h2 class="text-center">Captura tus ideas</h2>
        <div class="well">
            <h4>¿En que estás pensando?</h4>
            <div class="input-group">
                <input type="text" class="form-control input-sm" v-model="newIdea" maxlength="256">
                <span class="input-group-btn">
                    <a href="#" class="btn btn-primary btn-sm" v-on:click.prevent="createIdea">
                        Agregar
                    </a>
                </span>
            </div>        
            <hr>
            <ul class="list-unstyled">
                <li v-for="idea in ideas">                    
                    <p>
                        <small class="text-muted"><em>{{ idea.created_at }}</em></small> 
                        {{ idea.description }}
                    </p>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'
    import toastr from 'toastr'
    import moment from 'moment'

    
    
    export default {
        data () {
            return {
                ideas : [],
                newIdea: '',
            }
        },
        created: function() {
            this.getIdeas();
        },
        methods: {
            getIdeas: function(page) {
                var urlIdeas = 'mis-ideas';
                axios.get(urlIdeas).then(response => {
                    this.ideas = response.data
                });
            },
            createIdea: function() {
                var url = 'guardar-idea';
                axios.post(url, {
                    description: this.newIdea
                }).then(response => {
                    this.getIdeas();
                    this.newIdea = '';
                    toastr.success('Nueva idea registrada');
                });
            }
        }
    }
</script>
