<template>
  <div>

    <v-system-bar color="deep-purple darken-3" collapse></v-system-bar>
      
    <v-app-bar color="deep-purple accent-4" dark prominent dense >
      
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" class="nav"></v-app-bar-nav-icon>
      
      <v-toolbar-title >
        
      </v-toolbar-title>
        
      <v-spacer  class="titulo"> <v-btn  href="/produto"><h1>Movie Api</h1></v-btn> </v-spacer>

      <v-btn class="logbtn" href="/login">
        sign in
        <!-- <v-icon>mdi-magnify</v-icon> -->
      </v-btn>

      
      
    </v-app-bar>

    <v-navigation-drawer v-model="drawer" absolute bottom temporary dark >
      <v-list nav dense >
        <v-list-item-group
          v-model="group"
          active-class="white--text text--accent-4"
        >
          <v-list-item>
            <v-list-item-title
              ><router-link to="/produto"
                >Produto</router-link
              ></v-list-item-title
            >
          </v-list-item>

          <v-list-item>
            <v-list-item-title
              ><router-link to="/produtos"
                ><a>Produtos</a></router-link
              ></v-list-item-title
            >
          </v-list-item>
          <v-list-item>
            <v-list-item-title
              ><router-link to="/admin"
                >Admin</router-link
              ></v-list-item-title
            >
          </v-list-item>

          <v-list-item>
            <v-list-item-title
              ><router-link to="/Login">Login</router-link></v-list-item-title>
          </v-list-item>

        
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>
    <router-view> </router-view>
  </div>
</template>

<style>

.nav{
  margin-top: 20px;
}

a{
  color: #FFFFFF;
}

.logbtn{
  color: #FFFFFF;
  margin-top: 25px;
}

.titulo{
  display: flex;
  justify-content: center;
  margin-top: 30px;
  
}
</style>

<script>
export default {
  name: "App",
  data: () => ({
    drawer: false,
    group: null,
  }),

  watch: {
    group() {
      this.drawer = false;
    },
  },
};
</script>
