<template>
  <div class="container">
    <button @click="alerta()" type="button" class="btn btn-danger mb-3">Salir de la vista</button>
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
          v-model="prospecto.nombre"
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
          v-model="prospecto.apellidoPaterno"
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
          v-model="prospecto.apellidoMaterno"
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
          v-model="prospecto.calle"
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
          v-model="prospecto.numeroCasa"
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
          v-model="prospecto.colonia"
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
          v-model="prospecto.cp"
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
          v-model="prospecto.tel"
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
          v-model="prospecto.rfc"
          required
        />
      </div>

      <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label">Estatus</label>
        <select
          class="form-select"
          id="estatus"
          name="estatus"
          v-model="prospecto.estatus"
          required
        >
          <option selected>Selecciona un estatus</option>
          <option value="Enviado">Enviado</option>
        </select>
      </div>

      <div class="mb-3">
        <label for="formFileMultiple" class="form-label">Subir archivos</label>
        <input
          ref="upload"
          name="files"
          class="form-control"
          type="file"
          id="files"
          multiple
          required
        />
      </div>

      <button type="submit" class="btn btn-primary">Enviar Datos</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Formulario",

  methods: {
    alerta() {
      var mensaje;
      var opcion = confirm("si sale perderá toda la captura.");
      if (opcion == true) {
        this.$router.push("/listado");
      } else {
        mensaje = "Seguiremos en la vista actual";
      }
      document.getElementById("ejemplo").innerHTML = mensaje;
    },

    formdatasend() {
      const files = this.$refs.upload.files;
      const formData = new FormData();
      formData.append("nombre", this.prospecto.nombre);
      formData.append("apellidoPaterno", this.prospecto.apellidoPaterno);
      formData.append("apellidoMaterno", this.prospecto.apellidoMaterno);
      formData.append("calle", this.prospecto.calle);
      formData.append("numeroCasa", this.prospecto.numeroCasa);
      formData.append("colonia", this.prospecto.colonia);
      formData.append("cp", this.prospecto.cp);
      formData.append("tel", this.prospecto.tel);
      formData.append("rfc", this.prospecto.rfc);
      formData.append("estatus", this.prospecto.estatus);
      formData.append("observaciones", this.prospecto.observaciones);
      for (const file of files) {
        formData.append("files", file);
      }
      // formData.append("files", files,files);
      return formData;
    },
    submitForm() {
      axios
        .post("http://localhost:8081/agregar", this.formdatasend(), {
          headers: {
            // "contentType" : "application/json;charset=utf-8",
            // "Content-Type": "application/json",
            // "Accept": "application/json"
            "Content-Type": "multipart/form-data",
          },
        })
        .then(() => {
          //Perform Success Action
          alert("prospecto Agregado");
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

    // myFunction() {
    //   var d = document.getElementById("files").files;
    //   for (var i = 0; i < d.length; i++){
    //   console.log(d[i]);
    //   }

    //   }
  },
  data() {
    return {
      prospecto: {
        nombre: "",
        apellidoPaterno: "",
        apellidoMaterno: "",
        calle: "",
        numeroCasa: "",
        colonia: "",
        cp: "",
        tel: "",
        rfc: "",
        estatus: "",
        observaciones: "",
      },
    };
  },
};
</script>

