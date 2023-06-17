<template>
    <div class="container" >  
        <b-card>  
            <b-list-group v-for="entrega in entregas" :key="entrega.id">
              <b-list-group-item class="d-flex justify-content-between align-items-center">
                <div class="m-1 p-3">

                  {{ entrega.tipo_revision.nombre }}

                                      
                  </div>
                  <div class="d-flex position-absolute bottom-0 end-0 m-1" >
                      <div class="ml-auto"  v-if="entrega.calificacion =='en revision' ">
                          <b-button @click="detalleEntrega(entrega.id)"  variant="warning" class="m-1">
                            {{ entrega.calificacion }}
                          </b-button>
                      </div>
                      <div class="ml-auto"  v-else-if="entrega.calificacion=='aprobado'">
                          <b-button @click="detalleEntrega(entrega.id)" variant="info" class="m-1">
                            {{ entrega.calificacion }}
                          </b-button>
                      </div>
                      <div class="ml-auto"  v-else>
                          <b-button @click="detalleEntrega(entrega.id)" variant="danger" class="m-1">
                              {{ entrega.calificacion }}
                          </b-button>
                      </div>
                    </div> 
              </b-list-group-item>
          </b-list-group>
            <!-- <div class="text-end mt-3">
              <b-button variant="success" @click="crearEntrega(id)">Crear Entrega</b-button>
          </div> -->

        </b-card>
        <!-- <h3 >Entregas de {{ proyecto.nombre }}</h3>
        <b-card>
        </b-card> -->
    </div>
</template>

<script>
import axios from 'axios'

export default{
    name:'Lista',

    data(){
        return{
            id : this.$route.params.id,
            selectedOption: null,
            options: ['en revision','en desarrollo', 'terminado'],
            tipo:null,
            entregas:null
        }
    },
    methods:{
        selectOption(optionId) {
            if (this.selectedOption === optionId) {
              this.selectedOption = null; // Deseleccionar la opción actual si se hace clic nuevamente en ella
            } else {
              this.selectedOption = optionId; // Establecer la opción seleccionada
            //   this.editarProyecto()
            //   this.verProyecto()
            }
      },
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

