<template>
  <div class="container">
    <div class="row">
      <b-card>
        <div class="accordion" role="tablist" v-for="inscrito in inscritos" :key="inscrito.id">
          <b-card no-body class="mb-1">
            <b-card-header header-tag="header" class="p-1" role="tab">
              <b-button @click="getProyecto(inscrito.ficha.id)" block v-b-toggle="'accordion-' + inscrito.id" variant="info">Ficha: {{ inscrito.ficha.codigo }}</b-button>
            </b-card-header>
            <b-collapse :id="'accordion-' + inscrito.id" visible accordion="my-accordion" role="tabpanel">
              <b-card-body v-for="proyecto in proyecto" :key="proyecto.id">
                <b-card> 
                  <p class="card-text">{{proyecto.nombre_proyecto}}</p>
                  <b-button  v-b-toggle="'collapse-' + inscrito.id + '-' + proyecto.id + '-inner'" size="sm">descripcion</b-button>
                  <b-collapse :id="'collapse-' + inscrito.id + '-' + proyecto.id + '-inner'"  class="mt-2">
                    <b-card>{{ proyecto.descripcion }}</b-card>
                  </b-collapse>
                  <div class="text-end mt-3">
                    <b-button variant="success" @click="verProyecto(proyecto.id)">{{ proyecto.calificacion }}</b-button>
                  </div> 
                </b-card>
              </b-card-body>
            </b-collapse>
          </b-card>
        </div>
      </b-card>
    </div>
  </div>
</template>


<script>
import axios from 'axios'
  export default {
    data() {
      return {
        inscritos:null,
        proyecto:null
      }
    },
    methods:{
      getEntregas(id){
              axios.get("http://127.0.0.1:8000/api/fichas-instructor/"+id+'/').then(response=>{
              this.inscritos= response.data
            })
        },
      getProyecto(id){
        axios.get('http://127.0.0.1:8000/api/proyectos-instructor/'+id+'/').then(response=>{
          this.proyecto=response.data
        })
      },
      async verProyecto(id){
      this.$router.push('/detalle-proyecto/'+id)
    },

      toggleCollapse(inscritoId, proyectoId) {
        const collapseId = 'collapse-' + inscritoId + '-' + proyectoId + '-inner'
        this.$root.$emit('bv::toggle::collapse', collapseId)
      }
    },
    async mounted(){
      await this.getEntregas(3)
    }
  }
</script>