<template>
  <body id="page-top">
      <Navbar></Navbar>
      <div class="main">
      <div class="container-fluid">
        <div class="card shadow mb-4">
          <div class="card-header py-3">
            <h6 class="m-0 font-weight-bold text-primary">
              DataTables Kaprodi
            </h6>
          </div>
          <div class="card-body">
            <div class="table-responsive">
              <hr />
              <router-link :to="{ name: 'tambah_kaprodi' }">
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
                    <th>Nama Kaprodi</th>
                    <th>NIP</th>
                    <th>Prodi Yang diketuai</th>
                    <th>No hp</th>
                    <th>Option</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(kaprodi,index) in kaprodi" :key="index">
                    <td>{{ kaprodi.name_k }}</td>
                    <td>{{ kaprodi.nip }}</td>
                    <td>{{ kaprodi.prodi }}</td>
                    <td>{{ kaprodi.NOHP }}</td>
                    <td class="text-center">
                      <router-link
                        :to="{ name: 'edit_kaprodi', params: { id: kaprodi.id, index:index } }"
                        class="btn btn-sm btn-primary mr-1"
                        >EDIT</router-link
                      >
                      <button
                        @click.prevent="kaprodiDelete(kaprodi.id)"
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
    let kaprodi = ref([]);

    onMounted(() => {
      axios
        .get("https://ecowatchk10.000webhostapp.com/api/kaprodi")
        .then((response) => {
          kaprodi.value = response.data.kaprodi;
        })
        .catch((error) => {
          console.log(error.response.kaprodi);
        });
    });

    function kaprodiDelete(id,index) {
      axios
        .delete(`https://ecowatchk10.000webhostapp.com/api/kaprodi/${id}`)
        .then(() => {
          kaprodi.value.splice(index, 1);
        })
        .catch((error) => {
          console.log(error.response.data);
        });
    }

    return {
      kaprodi,
      kaprodiDelete,
    };
  },
};
</script>

<style src="../../../public/assets/css/style.css"></style>
