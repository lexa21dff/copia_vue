<!-- <template>
    <div>
      <div class="container">
        <b-card class="m-1 p-3">
          <div v-if="entrega">
            <div class="row">
              <div class="col-lg-1 col-md-1">
                <img class="imagen" src="../assets/2.jpg" alt="">
              </div>
              <div class="col-lg-9">
                <h3>{{ entrega.proyecto.nombre }}</h3>
                <p><span class="fw-lighter">Calificación: </span>{{ entrega.calificacion }}</p>
              </div>
            </div>
            <div class="row">
              <p class="fw-lighter">Descripción de la entrega:</p>
              <p>{{ entrega.descripcion_entrega }}</p>
            </div>
            <div class="row">
              <p class="fw-lighter">Respuesta del instructor:</p>
              <p>{{ entrega.respuesta_instructor }}</p>
            </div>
            <div class="row">
              <p class="fw-lighter">Instructor:</p>
              <p>{{ entrega.instructor }}</p>
            </div>
            <div class="row">
              <p class="fw-lighter">Autor:</p>
              <p>{{ entrega.autor }}</p>
            </div>
            <div class="row">
              <p class="fw-lighter">Creado:</p>
              <p>{{ entrega.creado }}</p>
            </div>
            <div class="row">
              <p class="fw-lighter">Editado:</p>
              <p>{{ entrega.editado }}</p>
            </div>
          </div>
          <div v-else>
            <p>Loading...</p>
          </div>
        </b-card>
      </div>
    </div>
  </template> -->
  
  <template>
    <div class="container" >
      <div class="accordion" role="tablist" v-for="entrega in entregas" :key="entrega.id">
        <b-card no-body class="mb-1">
          <b-card-header header-tag="header" class="p-1" role="tab">
            <b-button block v-b-toggle="'accordion-' + entrega.id"   variant="info">{{ entrega.tipo_revision.nombre }}</b-button>
          </b-card-header>
          <b-collapse :id="'accordion-' + entrega.id" visible accordion="my-accordion" role="tabpanel">
            <b-card-body>
              <b-card-text  class="fw-lighter">Descripcion:</b-card-text>
              <b-card-text>{{ entrega.descripcion_entrega }}</b-card-text>
            </b-card-body>
          </b-collapse>
        </b-card>
      </div>
    </div>
</template>

<script>
import axios from 'axios'

export default{
    name:'Lista',

    data(){
        return{
          id : this.$route.params.id,
            tipo:null,
            entregas:null
        }
    },
    methods:{
        async eliminarEntrega(id){
            console.log(id)
            await axios.delete("http://127.0.0.1:8000/api/entrega/"+id+'/')
            console.log(id)
            await this.getEntregas()
        },
        getEntregas(){
            let id = this.$route.params.id
              axios.get("http://127.0.0.1:8000/api/entregas/"+id+'/').then(response=>{
              this.entregas= response.data
            })
        },
        crearEntrega(id){
          console.log(id)
            this.$router.push('/crear-entrega/'+id)
        },
        detalleEntrega(id){
          console.log(id)
            this.$router.push('/detalle-entrega/'+id)
        },
        async getTipoRevision(id){
              axios.get("http://127.0.0.1:8000/api/tipo_revision/"+id+'/').then(response=>{
                this.tipo = response.data.nombre
                console.log(this.tipo)
                return this.tipo 
            })
        },   
    },
    async mounted(){

        await this.getEntregas()
    }
}
</script>
<style>
.imagen{
    width: 100%;
}

</style>