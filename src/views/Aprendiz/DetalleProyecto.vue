<template>
    <div>
      <body>
        <div v-if="rol !='aprendiz' ">

          <CalificarProyecto></CalificarProyecto>
          <CalificarEntrega></CalificarEntrega>
        </div>
        <div v-else>
          {{ rol }}
          <Proyecto></Proyecto>
          <Entrega></Entrega> 

        </div>
       </body>

    </div>
  </template>
  <script>
    import Entrega from '@/components/ListaDeEntregas.vue'
    import Proyecto from '@/components/DetalleProyecto.vue'
    import CalificarProyecto from '@/components/instructor/CalificarProyecto.vue'
    import CalificarEntrega from '@/components/instructor/ListaDeEntregas.vue'

   
  
  
    export default {
      data() {
          return {
              rol: this.$store.state.rol,
              
          }
      },
  
    components: {
      Proyecto,
      Entrega,
      CalificarProyecto,
      CalificarEntrega
    }
  
    }
  </script>