<template>
    <div class="row">
      <div class="col-12">
        <progressbar :porcentaje="porcentaje"/>
       </div>
    </div>

<div class="row">
    <form @submit.prevent="registrarproyecto" class="col-12 col-md-4">
        <div class="mb-3">
          <label  class="form-label">Proyecto</label>
          <input v-model.trim="proyecto" type="text" class="form-control" required>
        </div>
        <div class="mb-3">
            <label  class="form-label">Actividad</label>
            <select class="form-select" required  v-model.trim="tipo">
                <option disabled selected value="">Seleccione tipo de actividad</option>
                <option value="1">Aplicaciones Web con Vue.JS</option>
                <option value="2">Backend Service con Node.js</option>
                <option value="3">App Movil con React Native</option>
            </select>
        </div>
        <div class="mb-1">
            <label class="form-check-label">Urgente</label>
            <input v-model="urgente" type="checkbox" class="form-check-input m-1" v-model.trim="urgente">
        </div>
        <div class="mb-1">
            <button type="submit" class="btn btn-primary">Guardar</button>
        </div>
        
      </form>
     
      <div class="col-12 col-md-8">
      
        <total-proyectos 
        :numeroProyectos="numeroProyectos" 
        :proyectos="proyectos" 
        :cambiarEstado ="cambiarEstado" 
        :limpiarData="limpiarData"
        :Eliminar="Eliminar"/>
        
      </div>
</div>


</template>

<script>
    import Progressbar from './progressbar.vue';
    import TotalProyectos from './TotalProyectos.vue';
    export default{
        components: {
                Progressbar, TotalProyectos 
                },
        data: () => ({
            proyecto: "",
            tipo: "",
            urgente: false,
            proyectos: [],
            numeroProyectos: 0,
            
        }),
        methods: { 
            registrarproyecto() {
                const proyecto = {
                    proyecto: this.proyecto,
                    tipo: this.tipo,
                    urgente: this.urgente,
                    completado: false,
                };
                this.proyectos.push(proyecto);
                this.saveData();             
                this.numeroProyectos++;
                this.proyecto = "",
                this.tipo = "",
                this.urgente = false;
            },
            
       
            cambiarEstado(proyecto, campo) {
            //this.proyectos[id].urgente = !this.proyectos[id].urgente;
                    proyecto[campo]= !proyecto[campo];
            this.saveData();
            },
            saveData(){
                localStorage.getItem("proyectos", JSON.stringify(this.proyectos));
            },
           
            limpiarData (){
                this.proyectos= [];
                    localStorage.clear();
            },
            Eliminar(proyect){
                this.proyectos.splice(proyect,1);
            },
        },
            computed: {
            numeroProyectos() {
                return this.proyectos.length;
            },
            porcentaje() {
                let completados = 0;
                this.proyectos.map(proyecto => {
                    if (proyecto.completado)
                        completados++;
                });
                return (completados * 100) / this.numeroProyectos || 0;
                },
             },

            mounted (){ 
                this.proyectos = JSON.parse(localStorage.getItem("proyectos")) ||  [] ;
            },
        
};
</script>