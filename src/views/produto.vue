<template>
<div>
  <v-container>
      <input type="text" v-model="pesquisa" v-on:keyup.enter="procura(pesquisa)"> 
          <v-btn color="#29B6F6" :loading="loading1" @click="procura(pesquisa)">Search</v-btn>
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
                  <h3>Favoritos</h3>
                  <div v-for="(fav, index) in favoritos" :key="index">
                      {{fav.Title}} <v-icon x-small @click="removeFav(index)">mdi-close-circle-outline</v-icon>
                  </div>
                </div>

          </div>
<!-- Lista Produtos -->
    
      <div class="colunaProduct">
        <v-card min-width="450" max-width="450" min-height="866" max-height="1000"
            class="mx-auto my-12  " v-for="(item, index) in info.Search" :key="index">
            
                <v-card-title class="film" >
                  Title:  {{item.Title}}  {{info.Title}}<br>
                  Year: {{item.Year}}  {{info.Year}}<br>
                  Type: {{item.Type}}  {{info.Type}}<br> 
                  <v-img width="300" :src="info.Poster"></v-img>                  
                  </v-card-title>
                
                <v-card-text>
                  <v-btn color="#FDD835" id="link" href="#"  v-on:click.native='btnClick(item.imdbID)' target="_blank"> IMDB PAGE </v-btn>
                </v-card-text>
                
                <v-card-text max-width="450" min-height="670" class="poster">
                    <v-img v-if="item.Poster != 'N/A'" width="450" height="670" :src="item.Poster"></v-img>
                    <v-img v-else width="450" height="670" :src="not_found"></v-img>
              </v-card-text>

              <v-card-actions>
                <v-btn elevation="1" x-large depressed outlined color="#29B6F6" text @click="favorito(item)">
                  Guardar como Favorito
                </v-btn>
              </v-card-actions>
          </v-card >
      </div>
  </div>
        <div class="text-center" v-if="keep_search != null || info.totalResults >= 10 ">
        <v-container max-width="100" min-height="100">
            <v-row justify="center">
            <v-col>
                <v-container>
                <v-pagination v-model="page" class="my-1" :length="calculaPaginas(info.totalResults)" :total-visible="11" @input="carregaPagina"></v-pagination>
                </v-container>
            </v-col>
            </v-row>
        </v-container>
        </div>
  </v-container>
</div>
</template>


<style scoped>

.v-progress-circular {
  margin: 1rem;
}

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
      loading1: false,
      info: null,
      page: 1,
      favoritos: [],
      keep_search: null,
      not_found: "https://westsiderc.org/wp-content/uploads/2019/08/Image-Not-Available.png",
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
      .then(response => (this.info = response.data));
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
      this.loading1 = true
      this.keep_search = pesquisa
      axios.get("http://www.omdbapi.com/?s="+ encodeURIComponent(pesquisa) + "&page=1&apikey=47a567fc&" )
      .then(response => (this.info = response.data));
      this.loading1 = true
    },
  
  btnClick(id) {
      window.open("https://www.imdb.com/title/"+ id +"/");
    },
  beforeCreate: function(){
    document.body.className = 'Filmes'
  },
  
  carregaPagina(page){
    axios
    .get("http://www.omdbapi.com/?s="+encodeURIComponent(this.keep_search)+"&page="+page+"&apikey=47a567fc&")
    .then(response => (this.info = response.data));
  },

  calculaPaginas(resultados){
    var resto_divisao = resultados % 10
    var paginas = (resultados / 10) - (resto_divisao / 10)
    return paginas
  },
}
};
</script>
