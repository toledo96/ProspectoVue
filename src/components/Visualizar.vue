<template>
  <div class="container mb-5">
    <h2 class="text-center">Información de prospecto</h2>

    <div class="mb-3">
      <label for="exampleInputEmail1" class="form-label">Nombre</label>
      <input
        type="text"
        class="form-control"
        id="nombre"
        name="nombre"
        v-model="results.nombre"
        readonly
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
        readonly
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
        readonly
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
        readonly
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
        readonly
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
        readonly
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
        readonly
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
        readonly
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
        readonly
      />
    </div>

    <div class="mb-3">
      <label for="exampleInputPassword1" class="form-label">Estatus</label>
      <input
        type="text"
        class="form-control"
        id="estatus"
        name="estatus"
        v-model="results.estatus"
        readonly
      />
    </div>

    <div class="mb-3" v-show="results.estatus=='Rechazado'">
      <label for="exampleInputPassword1" class="form-label"
        >Observaciones</label
      >
      <textarea

        readonly
        class="form-control"
        v-model="results.observaciones"
        name="observaciones"
        id="observaciones"
        cols="30"
        rows="10"
      ></textarea>
    </div>

    <div class="mb-3">
      <label for="exampleInputPassword1" class="form-label">Documentos</label>
      <button
        class="form-control btn btn-link"
        v-for="archivo in archivos"
        v-bind:key="archivo.id"
        @click="DescargarArchivo(archivo.nombre, archivo.data, archivo.tipo)"
      >
        {{ archivo.nombre }}
      </button>
    </div>
  </div>
</template>
<script>
import axios from "axios";

export default {
  name: "Visualizar",

  created() {
    // console.log(this.$route);
    this.id = this.$route.params.id;
    // this.id = Number(this.id)
  },
  methods: {
    DescargarArchivo(name, base64Data, types) {
      var arrBuffer = this.base64ToArrayBuffer(base64Data);

      // Se crea un objeto blob con los datos del archivo y el tipo que se quiere
      var newBlob = new Blob([arrBuffer], { type: types });

      if (window.navigator && window.navigator.msSaveOrOpenBlob) {
        window.navigator.msSaveOrOpenBlob(newBlob);
        return;
      }

      //Se crea un link para el objetoURL que contiene el blob
      var data = window.URL.createObjectURL(newBlob);

      var link = document.createElement("a");
      document.body.appendChild(link);
      link.href = data;
      link.download = name;
      link.click();
      window.URL.revokeObjectURL(data);
      link.remove();
    },

    base64ToArrayBuffer(data) {
      var binaryString = window.atob(data);
      var binaryLen = binaryString.length;
      var bytes = new Uint8Array(binaryLen);
      for (var i = 0; i < binaryLen; i++) {
        var ascii = binaryString.charCodeAt(i);
        bytes[i] = ascii;
      }
      return bytes;
    },
  },
  data() {
    return {
      results: [],
      archivos: [],
    };
  },
  mounted: function () {
    axios.get("http://localhost:8081/prospecto/" + this.id).then((data) => {
      this.results = data.data;
      this.results.documentos.filter((value) => {
        this.archivos.push(value);
      });
      console.log(this.archivos);
    });
  },
};
</script>