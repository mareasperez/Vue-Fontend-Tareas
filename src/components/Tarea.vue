<template>
  <div>
    <h1 class="display-4 text-center">Listado de tareas</h1>
    <hr />
    <div class="row">
      <div class="col-lg-8 offset-lg-2">
        <div class="card mt-4">
          <div class="card-body">
            <div class="input-group">
              <input
                type="text"
                v-model="tarea"
                class="form-control form-control-lg"
                placeholder="Agregar Tarea"
              />
              <div class="input-group-append">
                <button
                  class="btn btn-success btn-lg"
                  v-on:click="agregarTarea()"
                >
                  Agregar
                </button>
              </div>
            </div>
            <br />
            <div class="text-center" v-if="loading">
              <div class="spinner-border text-success" role="status">
                <span class="sr-only">Loading...</span>
              </div>
            </div>
            <h5 v-if="listTareas.length == 0 && !loading">
              No hay tareas para mostrar
            </h5>
            <!-- {{ listTareas }} -->
            <ul class="list-group" v-if="!loading">
              <li
                v-for="(tarea, index) of listTareas"
                :key="index"
                class="list-group-item d-flex justify-content-between"
              >
                <span
                  class="cursor"
                  v-bind:class="{ 'text-success': tarea.estado }"
                >
                  <i
                    v-bind:class="[
                      tarea.estado ? 'fas fa-check-circle' : 'fal fa-circle',
                    ]"
                    v-on:click="editarEstado(tarea, tarea.id)"
                  ></i>
                </span>
                <!-- hacer la tarea -->
                {{ tarea.nombre }}
                <span
                  class="text-danger cursor"
                  v-on:click="eliminarTarea(tarea.id)"
                >
                  <i class="far fa-trash-alt"></i>
                </span>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
const url = "https://tareas-backend-api.azurewebsites.net/api/Tarea/";
export default {
  name: "Tarea",
  data() {
    return {
      tarea: "",
      listTareas: [],
      loading: false,
    };
  },
  methods: {
    agregarTarea() {
      const Tarea = {
        nombre: this.tarea,
        estado: false,
      };
      // this.listTareas.push(Tarea);
      this.loading = true;
      axios
        .post(url, Tarea)
        .then((response) => {
          console.log(response);
          this.obtenerTareas();
        })
        .catch((error) => {
          console.error(error);
        })
        .finally(() => {
          this.loading = false;
        });
      this.tarea = "";
    },
    eliminarTarea(id) {
      // this.listTareas.splice(index, 1);
      this.loading = true;
      axios
        .delete(url + id)
        .then((response) => {
          console.log(response);
          this.obtenerTareas();
          // this.loading = false;
        })
        .catch((error) => {
          console.error(error.message);
          // this.loading = false;
        })
        .finally(() => {
          this.loading = false;
        });
    },
    editarEstado(tarea, id) {
      // this.listTareas[index].estado = !tarea.estado;
      this.loading = true;
      axios
        .put(url + id, tarea)
        .then((response) => {
          console.log(response);
          this.obtenerTareas();
        })
        .catch((error) => {
          console.error(error);
        })
        .finally(() => {
          this.loading = false;
        });
    },
    obtenerTareas() {
      axios.get(url).then((response) => {
        // console.log(response.data);
        this.listTareas = response.data;
      });
    },
  },
  created: function() {
    this.obtenerTareas();
  },
};
</script>

<style scoped>
.cursor {
  cursor: pointer;
}
</style>
