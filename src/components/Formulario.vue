<template>

  <section class="src-components-formulario">
    <h2>Notas</h2>
    <hr>
    <vue-form :state="formState" @submit.prevent="enviar()">
      <validate tag="div">
        <label for="nombre" >Nombre</label>
        <input type="text" id="nombre" name="nombre" autocomplete="off" class="form-control" v-model.trim="formData.nombre" :minlength="nombreApellidoMinLength" :maxlength="nombreApellidoMaxLength" required no-espacios>
        <field-messages name="nombre" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Campo requerido</div>
            <div slot="no-espacios" class="alert alert-danger mt-1">No se permiten espacios en el campo</div>
            <div slot="minlength" class="alert alert-danger mt-1">Este campo requiere al menos {{ nombreApellidoMinLength }} caracteres</div>
            <div v-if="formData.nombre.length == nombreApellidoMaxLength" class="alert alert-danger mt-1">Este campo debe tener como máximo {{ nombreApellidoMaxLength }} caracteres</div>
          </field-messages>
      </validate>

      <validate tag="div">
        <label for="apellido" >Apellido</label>
        <input type="text" id="apellido" name="apellido" autocomplete="off" class="form-control" v-model.trim="formData.apellido" :minlength="nombreApellidoMinLength" :maxlength="nombreApellidoMaxLength" required no-espacios>
        <field-messages name="apellido" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Campo requerido</div>
            <div slot="no-espacios" class="alert alert-danger mt-1">No se permiten espacios en el campo</div>
            <div slot="minlength" class="alert alert-danger mt-1">Este campo requiere al menos {{ nombreApellidoMinLength }} caracteres</div>
            <div v-if="formData.apellido.length == nombreApellidoMaxLength" class="alert alert-danger mt-1">Este campo debe tener como máximo {{ nombreApellidoMaxLengths }} caracteres</div>
          </field-messages>
      </validate>

      <validate tag="div">
        <label for="nota" >Nota</label>
        <input type="number" id="nota" name="nota" autocomplete="off" class="form-control" v-model.number="formData.nota" :min="notaMin" :max="notaMax" required>
        <field-messages name="nota" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Campo requerido</div>
            <div slot="min" class="alert alert-danger mt-1">La nota mínima es de {{notaMin}}</div>
            <div slot="max" class="alert alert-danger mt-1">La nota máxima es de {{notaMax}}</div>
          </field-messages>
      </validate>
      <button class="btn btn-success mt-3 ml-3 mb-3" type="submit" :disabled="formState.$invalid">Enviar</button>
    </vue-form>

    
      <div v-if="notas.length" class="table-responsive">
        <div class="alert alert-secondary">Se encontraron {{ notas.length }} notas</div>
        <table class="table table-bordered">
          <thead>
            <tr>
              <th v-for="(col,index) in getCols" :key="index">{{ col }}</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(nota, index) in notas" :key="index">
              <td v-for="(col,index) in getCols" :key="index" >{{nota[col]}}</td>
            </tr>
            <tr>
              <td>Promedio</td>
              <td>{{ promedio }}</td>
            </tr>
          </tbody>
        </table>
      </div>
      <div v-else class="alert alert-danger">No se encontraron notas</div>
    
  </section>

</template>

<script lang="js">

  export default  {
    name: 'src-components-formulario',
    props: [],
    mounted () {

    },
    data () {
      return {
        formData : this.getInitialData(),
        formState : {},
        nombreApellidoMinLength : 3,
        nombreApellidoMaxLength : 15,
        notaMin : 0,
        notaMax : 10,
        notas : [],
        promedio : 0  // no llegue a hacerlo :'(
      }
    },
    methods: {
      getInitialData() {
        return {
          nombre: '',
          apellido: '',
          nota: ''
        }
      },

      enviar(){
        let alumno = {
          nombre: this.formData.nombre,
          apellido: this.formData.apellido,
          nota : this.formData.nota
        }
        console.log(alumno)
        
        this.notas.push(alumno)
        this.formData = this.getInitialData();
        this.formState._reset();
      },


      // getEstilos(nota) {
      //   let color
      //   if(nota < 3) {
      //     color : 'rojo'
      //   } 
      //   else if (nota >= 4 || nota <= 6) {
      //     color : 'yellow'
      //   }
      //   else {
      //     color : 'green'
      //   }
      //   return {
      //       color
      //   }
      // }

    },
    computed: {
      getCols() {
        return Object.keys(this.notas[0])
      }
    }
}


</script>

<style scoped lang="css">
  .src-components-formulario {

  }

  input {
    margin: 5px;
  }
</style>
