<template>
<div>
    <br>
    <h2 class="text-center justify-content-center">Captura tus ideas</h2>
    <div class="card bg-light">
        <div class="card-body">
            <h4 class="lead">¿En que estas pensando?</h4>
            <form v-on:submit.prevent="createIdea">
                <div class="input-group input-group-sm">
                    <input type="text" class="form-control input-sm" v-model="newIdea" maxlength="256">
                    <span class="input-group-btn">
                        <button type="submit" class="btn btn-primary btn-sm">
                             Agregar
                        </button>
                    </span>
                </div>
            </form>
            <hr>
            <ul class="list-unstyled">
                <li v-for="idea in ideas">
                    <p>
                        <small class="text-muted">
                            <em>{{ since(idea.created_at) }}</em>
                        </small>
                        {{ idea.description }}
                    </p>
                </li>
            </ul>
        </div>
    </div>
</div>
</template>

<script>
    import axios from 'axios'
    import toastr from 'toastr'
    import moment from 'moment'

    moment.locale('es');

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
           since: function(d) {
               return moment(d).fromNow();
           },
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
               }).catch(error => {
                   toastr.error('Error');
               });
           }
       }
   }
</script>
