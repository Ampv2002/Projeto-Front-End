<template >
    <v-container >
      <input type="text" v-model="pesquisa" v-on:keyup.enter="procura(pesquisa)" placeholder="Search by movie title">
          <v-btn color="#29B6F6" @click="procura(pesquisa)"  >Search</v-btn>
          <hr/>
        <div class="row">
         
        <!-- Lista favoritos -->
        <v-snackbar
      v-model="snackbar"
      :multi-line="multiLine"
    >
      {{ text }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="red"
          text
          v-bind="attrs"
          @click="snackbar = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>

            <div class="colunaFav">

                <div v-if="favoritos.length >0">
                  <h3>Favoritos </h3>
                  <div v-for="(fav, index) in favoritos" :key="index">
                      {{fav.Title}}{{info.Title}} <v-icon x-small @click="removeFav(index)">mdi-close-circle-outline</v-icon>
                  </div>
                </div>

          </div>
<!-- Lista Produtos -->
    
      <div class="colunaProduct">
        <v-card min-width="450" max-width="450" min-height="866" max-height="1000"
            class="mx-auto my-12  " v-for="(item, index) in info" :key="index">
                <v-card-title class="film" >
                  Titulo:  {{item.Title}}  {{info.Title}}<br>
                  Year: {{item.Year}}  {{info.Year}}<br>
                  Type: {{item.Type}}  {{info.Type}}<br>    
                  <v-img width="450" :src="info.Poster"></v-img>               
                  </v-card-title>
                
                <v-card-text>
                 <v-btn color="#FDD835" id="link" href="#"  v-on:click.native='btnClick(item.imdbID,info.imdbID)' target="_blank"> IMDB PAGE </v-btn>
                </v-card-text>
               
                <v-card-text max-width="450" min-height="670" class="poster">
                    <v-img  v-if="item.Poster != 'N/A'" width="450" height="670" :src="item.Poster"></v-img>
                    <v-img v-else width="450" height="670" :src="not_found" ></v-img>
              </v-card-text>

              <v-card-actions>
                <v-btn elevation="15" outlined color="#29B6F6" text @click="favorito(item)">
                  Guardar como Favorito
                </v-btn>
              </v-card-actions>
          </v-card >
      </div>
  </div>
   </v-container>
</template>

<style>
.admin{
    background-color: lightgrey;

}

.Filmes{
  font-family: Calibre, "San Francisco", "SF Pro Text", -apple-system, system-ui, BlinkMacSystemFont, Roboto, "Helvetica Neue", "Segoe UI", Arial, sans-serif;
  background-color: lightgrey;
}

.poster{
  display: flex;
  justify-content: center;
}

.row {
  display: block;
  margin: auto;
}


.colunaProduct {
  flex: 80%;
  padding: 10px;
  display: grid;
  gap: 4px;
  align-items: center;
  justify-items: center;
  grid-template-columns: auto auto auto;
  grid-row-gap: 100px;
}

.film {

  text-transform: capitalize;
  display: table;
  table-layout: fixed;
  width: 100%;
  word-break: break-word;
  flex-grow: 1;
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
}

.colunaFav {
  flex: 40%;
  padding: 10px;
}

.v-btn:hover {
  text-decoration: underline;
  
}

</style>



<script>

import axios from "axios";

export default {
  data() {
    return {
      not_found: "https://westsiderc.org/wp-content/uploads/2019/08/Image-Not-Available.png" , 
      info: null,
      favoritos: [],
      vertical: true,
      multiLine: true,
      snackbar: false,
      text: `Esse item já foi adicionado aos favoritos`,
    };
  },
  mounted() {
    var that = this;
    axios
      .get("https://projeto-frontend-default-rtdb.europe-west1.firebasedatabase.app/.json")
      .then(response => (this.info = response.data.movies));
    console.log(that.info);
  },
  methods: {
    favorito(item) {
      if (this.favoritos.indexOf(item ) === -1){
        this.favoritos.push(item);
        
      }else {
        this.snackbar = true;
      }
      console.log(this.favoritos);
    },

    removeFav(item){
      this.favoritos.splice(item, 1)

    },

    procura(pesquisa) {
      axios.get("http://www.omdbapi.com/?s="+ encodeURIComponent(pesquisa) + "&apikey=47a567fc&" )
      .then(response => (this.info = response.data.Search));
    },

   
    btnClick(id) {
      window.open("https://www.imdb.com/title/"+ id +"/");
    }

  },
  beforeCreate: function(){
    document.body.className = 'Filmes'
  }
};
</script>
