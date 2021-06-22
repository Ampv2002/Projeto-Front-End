<template>
    <div>
        <div class="colunaProduct">
        <v-card min-width="450" max-width="450" min-height="866" max-height="866"
            class="mx-auto my-12  " v-for="(item, index) in info.Search" :key="index">
            
                <v-card-title class="film" >
                  Titulo:  {{item.Title}} <br>
                  Year: {{item.Year}} <br>
                  Type: {{item.Type}} <br>                 
                  </v-card-title>
                <!--
                <v-card-text>
                  IMDB: <v-btn id="link" href="#" @click:src="imdb(item.imdbID)" target="_blank"> IMDB PAGE </v-btn>
                </v-card-text>
                -->
                <v-card-text max-width="450" min-height="670" class="poster">
                    <v-img width="450" height="670" :src="item.Poster"></v-img>
              </v-card-text>

              <v-card-actions>
                <v-btn color="deep-purple " text @click="favorito(item)">
                  Guardar como Favorito
                </v-btn>
              </v-card-actions>
          </v-card >
      </div>
      <v-app>
        <div class="text-center">
        <v-container>
            <v-row justify="center">
            <v-col cols="8">
                <v-container class="max-width">
                <v-pagination v-model="page" class="my-4" :length="calculaPaginas(info.totalResults)" :total-visible="7" @input="carregaPagina"></v-pagination>
                </v-container>
            </v-col>
            </v-row>
        </v-container>
        <Footer_default/>
        </div>
    </v-app>
    </div>

</template>



<style>
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
  flex-grow: 1;
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
}

.colunaFav {
  flex: 40%;
  padding: 10px;
}

</style>

<script>
import axios from "axios";
import Footer_default from '@/components/Footer_Root.vue' 

export default {
  name: 'produtos',
    components: {
    Footer_default
  },
  data() {
    return{
      info: '',
      page: 1,
    }
  },

  mounted(){
    axios
    .get("http://www.omdbapi.com/?s=Batman&page=1&apikey=47a567fc&")
    .then(response => (this.info = response.data));
  },
  methods:{
    carregaPagina(page){
      axios
      .get("http://www.omdbapi.com/?s=Batman&page="+page+"&apikey=47a567fc&")
      .then(response => (this.info = response.data));
    },
    calculaPaginas(resultados){
      var resto_divisao = resultados % 10
      var paginas = (resultados / 10) - (resto_divisao / 10)
      return paginas
    }
  },
};
</script>
