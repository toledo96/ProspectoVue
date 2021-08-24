<template>
  <div class="container">
    <div class="card border-primary mb-3" style="max-width: 100%">
      <div class="card-header">Prospectos</div>
      <div class="card-body text-primary">
        <h5 class="card-title">Listado de prospectos</h5>

        <table class="table table-bordered table-striped">
          <thead>
            <tr>
              <th>Nombre</th>
              <th>Apellido Paterno</th>
              <th>Apellido Materno</th>
              <th>Estatus</th>
              <th>Acciones</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="result in results" v-bind:key="result.id">
              <td>{{ result.nombre }}</td>
              <td>{{ result.apellidoPaterno }}</td>
              <td>{{ result.apellidoMaterno }}</td>
              <td>{{ result.estatus }}</td>
              <td>
                <router-link v-bind:to="'/visualizar/' + result.id">
                  <button class="btn-primary">Visualizar</button>
                </router-link>
                <router-link v-bind:to="'/evaluar/' + result.id">
                  <button class="btn-danger">Evaluar</button>
                </router-link>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Listado",

  methods: {},
  data() {
    return {
      results: [],
    };
  },
  mounted: function () {
    axios.get("http://localhost:8081/listado").then((data) => {
      this.results = data.data;

    });
  },
};
</script>

