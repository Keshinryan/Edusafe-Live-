<template>
  <body>
    <div id="wrapper">
      <Navbar></Navbar>
      <div class="container-fluid">
        <h1 class="h3 mb-4 text-gray-800">Data Edukasi</h1>
        <div class="row justify-content-center">
          <div class="col-md-8">
            <div class="card">
              <div class="card-header justify-content-center">
                Form Edit Data Edukasi
              </div>
              <div class="card-body">
                <form @submit.prevent="update" enctype="multipart/form-data">
                  <div class="form-group">
                    <label for="judul">Judul Content</label>
                    <input type="text" name="judul" class="form-control" v-model="edukasi.judul"
                      placeholder="Judul Content" />
                    <div v-if="validation.judul" class="mt-2 alert alert-danger">
                      {{ validation.judul[0] }}
                    </div>
                  </div>
                  <div class="form-group">
                    <label for="isi">Isi Content</label>
                    <textarea type="text" name="isi" class="form-control" v-model="edukasi.isi"
                      placeholder="Isi Content"></textarea>
                    <div v-if="validation.isi" class="mt-2 alert alert-danger">
                      {{ validation.isi[0] }}
                    </div>
                  </div>
                  <div class="form-group" @submit.prevent="updateFile()">
                    <label for="foto">Gambar</label>
                    <input type="file" ref="fileInput" name="foto" class="form-control" id="foto" placeholder="foto"
                      @change="handleFileUpload($event)" />
                    <img :src="getFileUrl(edukasi.foto)" alt="" v-if="edukasi.foto" style="height: 100px" />
                    <div v-if="validation.foto" class="mt-2 alert alert-danger">
                      {{ validation.foto[0] }}
                    </div>
                  </div>
                  <router-link :to="{ name: 'data_edukasi' }" class="btn btn-danger">Tutup</router-link>
                  <button type="submit" name="tambah" class="btn btn-primary float-right">
                    Edit Edukasi
                  </button>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </body>
</template>

<script>
import { reactive, ref, onMounted } from "vue";
import { useRouter, useRoute } from "vue-router";
import axios from "axios";

export default {
  setup() {
    const edukasi = reactive({
      judul: "",
      isi: "",
      foto: null,
    });

    const route = useRoute();

    const validation = ref([]);
    const router = useRouter();

    onMounted(() => {
      axios
        .get(`https://ecowatchk10.000webhostapp.com/api/edukasi/${route.params.id}`)
        .then((response) => {
          edukasi.judul = response.data.data.judul;
          edukasi.isi = response.data.data.isi;
          edukasi.foto = response.data.data.foto;
        })
        .catch((error) => {
          console.log(error.response.data);
        });
    });
    function updateFile() {
      let foto= edukasi.foto;
      let formData = new FormData();
      formData.append("foto", foto);
      console.log(foto);
      axios
        .post(`https://ecowatchk10.000webhostapp.com/api/file/${route.params.id}`, formData, {
          headers: {
            "Content-Type": "multipart/form-data",
          },
        })
        .catch((error) => {
          validation.value = error.response.data;
        });
    }

    function update() {
      let judul = edukasi.judul;
      let isi = edukasi.isi;
      let formData = new URLSearchParams();
      formData.append("judul", judul);
      formData.append("isi", isi);

      axios
        .put(`https://ecowatchk10.000webhostapp.com/api/edukasi/${route.params.id}`, formData.toString(), {
          headers: {
            "Content-Type": "application/x-www-form-urlencoded",
          },
        })
        .then(() => {
          router.push({
            name: "data_edukasi",
          });
        })
        .catch((error) => {
          validation.value = error.response.data;
        });
        this.updateFile();
    }

    function getFileUrl(filename) {
      return `https://ecowatchk10.000webhostapp.com/api/file/${filename}/foto`;
    }
    function handleFileUpload(event) {
      edukasi.foto = event.target.files[0];
    }
    return {
      edukasi,
      validation,
      router,
      update,
      getFileUrl,
      handleFileUpload,
      updateFile,
    };
  },
};
</script>

<style></style>
