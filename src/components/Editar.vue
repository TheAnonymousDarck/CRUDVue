<template>
    <div class="container">
        <div class="card">
            <div class="card-header">
                Actualizar empleados
            </div>
            <div class="card-body">
                
                <form v-on:submit.prevent="actualizarRegistro" >

                    <div class="form-group">
                        <label for="nombre">nombre</label>
                        <input type="text" required name="nombre" v-model="empleado.nombre" id="nombre" 
                        class="form-control" placeholder="nombre" aria-describedby="helpId">
                        <small id="helpId" class="text-muted">Escribe el nombre del empleado</small>
                    </div>

                    <div class="form-group">
                        <label for="email">email</label>
                        <input type="email" required name="correo" v-model="empleado.correo" id="correo" 
                        class="form-control" placeholder="correo" aria-describedby="helpId">
                        <small id="helpId" class="text-muted">agrega correo</small>
                    </div>

                    <div class="btn-group" role="group" aria-label="">
                        <button type="submit" class="btn btn-success">Modificar</button>
                        <router-link :to="{name:'Listar'}" class="btn btn-warning">Cancelar</router-link>
                    </div>

                </form>

            </div>
        </div>
    </div>
</template>


<script>
export default {
    data() {
        return {
            empleado: {}
        };
    },
    created:function(){ 
        this.obtenerInformationID();
    },
    methods:{
        obtenerInformationID(){ 
            
            fetch('http://localhost/empleados/?consultar='+this.$route.params.id)
            .then(respuesta=>respuesta.json())
            .then((datosRespuesta)=>{ 
                console.log(datosRespuesta);
                this.empleado = datosRespuesta[0];
            })
            .catch(console.log)
        },
        actualizarRegistro(){
            var datosEnviar={
                id:this.$route.params.id,
                nombre: this.empleado.nombre,
                correo: this.empleado.correo
            }
            
            fetch('http://localhost/empleados/?actualizar='+this.$route.params.id,{
                method: 'POST',
                body:JSON.stringify(datosEnviar)
            })
            .then(respuesta=>respuesta.json())
            .then((datosRespuesta=>{ 
                console.log(datosRespuesta);
                window.location.href='../listar'
            }))
        }

    }
}
</script>