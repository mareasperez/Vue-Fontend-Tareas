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
            <h5 v-if="listTareas.length == 0">
              No hay tareas para mostrar
            </h5>
            <!-- {{ listTareas }} -->
            <ul class="list-group">
              <li
                v-for="(tarea, index) of listTareas"
                :key="index"
                class="list-group-item d-flex justify-content-between"
              >
                <span
                  class="cursor"
                  v-bind:class="{ 'text-success': tarea.estado }"
                  ><i
                    v-bind:class="[
                      tarea.estado ? 'fas fa-check-circle' : 'fal fa-circle',
                    ]"
                    v-on:click="editarEstado(tarea, index)"
                  ></i>
                </span>
                <!-- hacer la tarea -->
                {{ tarea.nombre }}
                <span
                  class="text-danger cursor"
                  v-on:click="eliminarTarea(index)"
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
export default {
  name: "Tarea",
  data() {
    return {
      tarea: "",
      listTareas: [],
    };
  },
  methods: {
    agregarTarea() {
      const Tarea = {
        nombre: this.tarea,
        estado: false,
      };
      this.listTareas.push(Tarea);
      this.tarea = "";
    },
    eliminarTarea(index) {
      this.listTareas.splice(index, 1);
    },
    editarEstado(tarea, index) {
      this.listTareas[index].estado = !tarea.estado;
    },
  },
};
</script>

<style scoped>
.cursor {
  cursor: pointer;
}
</style>
