<template>
    <div>
      <body>
        <Proyecto></Proyecto>
        <Entrega></Entrega> 
        <CalificarProyecto></CalificarProyecto>
        <CalificarEntrega></CalificarEntrega>
       </body>

    </div>
  </template>
  <script>
    import Entrega from '@/components/ListaDeEntregas.vue'
    import Proyecto from '@/components/DetalleProyecto.vue'
    import CalificarProyecto from '@/components/CalificarProyecto.vue'
    import CalificarEntrega from '@/components/Calificar_entrega.vue'

   
  
  
    export default {
  
      components: {
    Proyecto,
    Entrega,
    CalificarProyecto,
    CalificarEntrega
}
  
    }
  </script>