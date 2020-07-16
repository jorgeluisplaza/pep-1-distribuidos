<template>
  <div class="col-md-4 col-md-offset-4 center_div">
    <br>
    <div>
      <h1>Solicitud de permiso</h1>
    <form>
      <div class="form-group">
        <label>Ingrese su rut</label>
        <input class="form-control" v-model="rut" id="rut" placeholder="12345678-9">
      </div>
      <div class="form-group">
        <label>Nombre completo</label>
        <input class="form-control" v-model="name" id= "nombre" placeholder="Nombre">
      </div>
      <div class="form-group">
        <label>Direccion</label>
        <input class="form-control" v-model="direccion" id="direccion" placeholder="Direccion">
      </div>
      <div class="form-group">
        <label>Motivo del permiso</label>
        <select v-model="duracion" @change="onChange" class="form-control">
          <option v-for="option in opciones" v-bind:value="option.value">
            {{ option.motivo }}
          </option>
        </select>
      </div>
      <label v-if="change">Duración del permiso: {{ duracion }} minutos</label>
      <br>
      <button type="submit" @click="onSubmit" data-toggle="modal" data-target="#exampleModal" class="btn btn-primary">Solicitar</button>

      <div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="exampleModalLabel">Se ha generado su permiso correctamente</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body">
              <label>Id del permiso: {{ id }}</label>
              <br>
              <label>Fecha inicio del permiso: {{ emittedAt }}</label>
              <br>
              <label>Fecha expiracion del permiso: {{ validExtension }}</label>
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-danger" data-dismiss="modal">Cerrar</button>
            </div>
          </div>
        </div>
      </div>
      <hr>
    </form>
    </div>
  </div>
</template>

<script>
    import axios from 'axios';
    export default {
        data() {
          return {
            showAlert: false,
            rut: '',
            name: '',
            direccion: '',
            motivo: '',
            id: '',
            emittedAt: '',
            validExtension: '',
            duracion: '',
            change: false,
            opciones: [
              {motivo: 'Paseo de mascotas', value: '30'},
              {motivo: 'Compras de insumos básicos', value: '180'},
              {motivo: 'Pago de servicios básicos', value: '180'},
              {motivo: 'Entrega de insumos a adultos mayores', value: '120'}
            ]
          }
        },
        methods: {
            onSubmit() {
              const formData = {
                run: this.rut,
                name: this.name,
                address: this.direccion,
                cause: this.motivo,
                minutesExtension: this.duracion,
              }
              axios.post('http://165.227.1.248:8090/licence/', formData)
                .then(res => {
                  const data = res.data;
                  this.id = data.id;
                  this.emittedAt = data.emittedAt;
                  this.validExtension = data.validExtension;
                })
                .catch(error => console.log(error));
                this.showAlert = true;
                this.rut = '';
                this.name = '';
                this.direccion = '';
                this.motivo = '';
                this.duracion = '';
                this.change = false;
            },
            onChange() {
              this.change = true;
            }
        }
    }
</script>

<style scoped>
  .center_div{
    margin: 0 auto;
    width:80% /* value of your choice which suits your alignment */
  }
</style>
