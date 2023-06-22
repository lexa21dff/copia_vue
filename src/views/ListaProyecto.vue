<template>
  <div>
    <div class="container">
      <div class="row">
        <b-input-group size="sm" class="mb-2">
          <b-form-input type="search" v-model="searchValue" placeholder="Search terms" @search-clear="clearSearch"></b-form-input>
          <b-input-group-prepend is-text>
            <b-icon icon="search" @click="search"></b-icon>
          </b-input-group-prepend>
        </b-input-group>
        {{ searchValue }}
        <b-nav-form>
            <!-- <b-form-input type="text" v-model="searchValue" size="sm" class="mr-sm-2" placeholder="Search" ></b-form-input>
            <b-button size="sm" class="my-2 my-sm-0"  @click="search">Buscar</b-button> -->
            <div class="listaPyoyecto" v-for="proyecto in proyecto" :key="proyecto.id">
                <h2>{{ proyecto.nombre_proyecto }}</h2>
                <td>
                  <p>{{ proyecto.descripcion }}</p>

                </td>
                <td>
                  
                  <img class="verProyecto" src="../assets/iconos/verProyecto.png" alt="">

                </td>

            </div>
        </b-nav-form>
      </div>
      <div class="row">
        <div class="container">
          <table>
            <tbody>
              <tr v-for="proyecto in proyectos" :key="proyecto.id">
                <b-card class="m-1 p-3">
                  <div class="row">
                    <div class="col-lg-1 col-md-1">
                      <img class="imagen" src="../assets/2.jpg" alt="">
                    </div>
                    <div class="col-lg-9">
                      <h3>{{ proyecto.nombre_proyecto }}</h3>
                      <p><span class="fw-lighter">Estado: </span>{{ proyecto.estado }}</p>
                    </div>
                  </div>
                  <div class="row">
                    <p class="fw-lighter">Descripcion:</p>
                    <p>{{ descripcion(proyecto.descripcion) }}</p>
                  </div>
                  <img class="position-absolute bottom-0 end-0" src="../assets/iconos/verProyecto.png" alt="" @click="verProyecto(proyecto.id)">
                </b-card>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

  
  
  <script>
  import axios from 'axios'

  export default{
      name:'Lista',
      data(){
          return{
              searchValue: null,
              proyecto: [],
              proyectos: null
          }
      },
      methods:{
          getProyecto(){
              axios.get("http://127.0.0.1:8000/api/proyecto/").then(response=>{
              this.proyectos= response.data
            })
          },
          descripcion(descripcion){
            // descripcion corta
            if (descripcion.length > 100) {
              const index = descripcion.slice(100).search(/[.]/);
                if (index !== -1) {
                  return descripcion.slice(0, 100 + index + 1) + '...';
                }
            }
            return descripcion;
          },
          
      async verProyecto(id){
        this.$router.push('/ver-proyecto/'+id)
      },
      clearSearch() {
        this.getProyecto()
        console.log('entro')
        this.searchValue = null;
    },
  
      async search() {
      try {
        const response = await axios.get("http://127.0.0.1:8000/buscar_proyectos/", {
          params: {
            search: this.searchValue,
          },
        });
        this.proyectos = response.data;
      } catch (error) {
        console.log(error);
      }
      
    },
  
  
      },
      mounted() {  
        this.getProyecto()
  
      },
      watch: {
        searchValue(newValue) {
          if (newValue === null || newValue === "") {
            this.getProyecto();
          } else {
            this.search();
          }
        }
        
      }

  
  }
  </script>

<style>

.imagen{
    width: 100%;
}

</style>