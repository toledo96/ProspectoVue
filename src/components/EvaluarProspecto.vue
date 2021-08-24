<template>
  <div class="container">
    <form
      @submit.prevent="submitForm"
      class="mb-5"
      method="POST"
      enctype="multipart/form-data"
    >
      <h2 class="text-center">Registro Prospectos</h2>

      <div class="mb-3">
        <label for="exampleInputEmail1" class="form-label">Nombre</label>
        <input
          type="text"
          class="form-control"
          id="nombre"
          name="nombre"
          v-model="results.nombre"
          required
        />
      </div>

      <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label"
          >Apellido Paterno</label
        >
        <input
          type="text"
          class="form-control"
          id="apellidoPaterno"
          name="apellidoPaterno"
          v-model="results.apellidoPaterno"
          required
        />
      </div>

      <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label"
          >Apellido Materno</label
        >
        <input
          type="text"
          class="form-control"
          id="apellidoMaterno"
          name="apellidoMaterno"
          v-model="results.apellidoMaterno"
          required
        />
      </div>

      <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label">Calle</label>
        <input
          type="text"
          class="form-control"
          id="calle"
          name="calle"
          v-model="results.calle"
          required
        />
      </div>

      <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label"
          >Número de casa</label
        >
        <input
          type="text"
          class="form-control"
          id="numeroCasa"
          name="numeroCasa"
          v-model="results.numeroCasa"
          required
        />
      </div>

      <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label">Colonia</label>
        <input
          type="text"
          class="form-control"
          id="colonia"
          name="colonia"
          v-model="results.colonia"
          required
        />
      </div>

      <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label">CP</label>
        <input
          type="text"
          class="form-control"
          id="cp"
          name="cp"
          v-model="results.cp"
          required
        />
      </div>

      <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label">Tel</label>
        <input
          type="text"
          class="form-control"
          id="tel"
          name="tel"
          v-model="results.tel"
          required
        />
      </div>

      <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label">RFC</label>
        <input
          type="text"
          class="form-control"
          id="rfc"
          name="rfc"
          v-model="results.rfc"
          required
        />
      </div>

      <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label">Estatus</label>
        <select
          class="form-select"
          id="estatus"
          name="estatus"
          v-model="results.estatus"
          required
        >
          <option selected>{{results.estatus}}</option>
          <option value="Autorizado">Autorizado</option>
          <option value="Rechazado">Rechazado</option>
        </select>
      </div>

      <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label">Observaciones</label>
          <textarea required :disabled="results.estatus== 'Autorizado' || results.estatus== 'Enviado' " class="form-control" v-model="results.observaciones" name="observaciones" id="observaciones" cols="30" rows="10"></textarea>
      </div>

      <button type="submit" class="btn btn-primary">Actualizar Datos</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "EvaluarProspecto",

  created() {
    console.log(this.$route);
    this.id = this.$route.params.id;
    // this.id = Number(this.id)
  },
  methods: {
    formdatasend() {
      const formData = new FormData();
      formData.append("nombre", this.results.nombre);
      formData.append("apellidoPaterno", this.results.apellidoPaterno);
      formData.append("apellidoMaterno", this.results.apellidoMaterno);
      formData.append("calle", this.results.calle);
      formData.append("numeroCasa", this.results.numeroCasa);
      formData.append("colonia", this.results.colonia);
      formData.append("cp", this.results.cp);
      formData.append("tel", this.results.tel);
      formData.append("rfc", this.results.rfc);
      formData.append("estatus", this.results.estatus);
      formData.append("observaciones", this.results.observaciones);
      return formData;
    },
    submitForm() {
      axios
        .put(
          "http://localhost:8081/prospecto/editar/" + this.id,
          this.formdatasend(),
          {
            headers: {
              "Content-Type": "multipart/form-data",
            },
          }
        )
        .then(() => {
          //Perform Success Action
          alert("prospecto Evaluado");
          this.$router.push("/listado");
        })
        .catch((e) => {
          // error.response.status Check status code
          console.log(e);
        })
        .finally(() => {
          //Perform action in always
        });
    },
  },
  data() {
    return {
      results: [],
    };
  },
  mounted: function () {
    axios.get("http://localhost:8081/prospecto/" + this.id).then((data) => {
      this.results = data.data;
      console.log(data.data);
    });
  },
};
</script>

