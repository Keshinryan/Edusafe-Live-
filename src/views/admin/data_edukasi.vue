<template>
  <body id="page-top">
      <Navbar></Navbar>
      <div class="main">
        <div class="container-fluid">
        <div class="card shadow mb-4">
          <div class="card-header py-3">
            <h6 class="m-0 font-weight-bold text-primary">
              DataTables Edukasi
            </h6>
          </div>
          <div class="card-body">
            <div class="table-responsive">
              <hr />
              <router-link :to="{ name: 'tambah_edukasi' }">
                <button class="btn btn-sm btn-success">Tambah Data</button>
              </router-link>
              <hr />
              <table
                class="table table-bordered"
                id="dataTable"
                width="100%"
                cellspacing="0"
              >
                <thead>
                  <tr>
                    <th>Judul</th>
                    <th>Isi</th>
                    <th>Gambar</th>
                    <th>Option</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(edukasi,index) in edukasi" :key="index">
                    <td>{{ edukasi.judul }}</td>
                    <td>{{ edukasi.isi }}</td>
                    <td><img :src="getFileUrl(edukasi.foto)" alt="" v-if="edukasi.foto" style="height: 300px; ;"></td>
                    <td class="text-center">
                      <router-link
                        :to="{ name: 'edit_edukasi', params: { id: edukasi.id} }"
                        class="btn btn-sm btn-primary mr-1"
                        >EDIT</router-link
                      >
                      <button
                        @click.prevent="edukasiDelete(edukasi.id, index)"
                        class="btn btn-sm btn-danger ml-1"
                      >
                        DELETE
                      </button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- /.container-fluid -->
  </body>
</template>

<script>
import axios from "axios";
import { onMounted, ref } from "vue";

export default {
  setup() {
    const edukasi = ref([]);
    onMounted(() => {
      axios
        .get("https://ecowatchk10.000webhostapp.com/api/edukasi")
        .then((response) => {
          edukasi.value = response.data.data;
        })
        .catch((error) => {
          console.log(error.response.data);
        });
    });
    function getFileUrl(filename){
      return `https://ecowatchk10.000webhostapp.com/api/file/${filename}/foto`;
    }
    function edukasiDelete(id,index) {
      axios
        .delete(`https://ecowatchk10.000webhostapp.com/api/edukasi/${id}`)
        .then(() => {
          edukasi.value.splice(index, 1);
        })
        .catch((error) => {
          console.log(error.response.data);
        });
    }

    return {
      edukasi,
      edukasiDelete,
      getFileUrl,
    };
  },
};
</script>

<style src="../../../public/assets/css/style.css"></style>
