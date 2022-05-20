<template>
  <div class="keranjang">
    <Navbar :updateKeranjang="keranjangs"/>
    <div class="container">
      <div class="row mt-5">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/" class="text-dark">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods" class="text-dark">Foods</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">
                Keranjang
              </li>
            </ol>
          </nav>
        </div>
      </div>
      <div class="row">
        <div class="col">
          <div class="table-responsive mt-3">
            <table class="table">
              <thead>
                <tr>
                  <th scope="col">#</th>
                  <th scope="col">Foto</th>
                  <th scope="col">Makanan</th>
                  <th scope="col">Keterangan</th>
                  <th scope="col">Jumlah</th>
                  <th scope="col">Harga</th>
                  <th scope="col">Total Harga</th>
                  <th scope="col">Hapus</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(keranjang, index) in keranjangs"
                  :key="keranjang.products.id"
                >
                  <th scope="row">{{ index + 1 }}</th>
                  <td>
                    <img
                      v-if="keranjang.products.gambar"
                      :src="
                        require(`@/assets/images/${keranjang.products.gambar}`)
                      "
                      class="img-fluid shadow"
                      width="250"
                    />
                  </td>
                  <td>{{ keranjang.products.nama }}</td>
                  <td>
                    {{ keranjang.keterangan ? keranjang.keterangan : "-" }}
                  </td>
                  <td>{{ keranjang.jumlah_pemesanan }}</td>
                  <td align="right">Rp. {{ keranjang.products.harga }}</td>
                  <td align="right">
                    <strong>
                      Rp.
                      {{
                        keranjang.products.harga * keranjang.jumlah_pemesanan
                      }}</strong
                    >
                  </td>
                  <td
                    align="center"
                    class="text-danger"
                    @click="
                      deleteKeranjangs(keranjang.id, keranjang.products.nama)
                    "
                  >
                    <b-icon-trash></b-icon-trash>
                  </td>
                </tr>
                <tr>
                  <td colspan="6" align="right">
                    <strong>Total Harga: </strong>
                  </td>
                  <td align="right">{{ totalHarga }}</td>
                  <td></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>

  <div class="row justify-content-end">
    <div class="col-md-4">
    <form action="" class="mt-4" v-on:submit.prevent>
            <div class="form-group">
              <label for="nama">Nama</label>
              <input
                type="text"
                class="form-control"
                v-model="pesan.nama"
              />
            </div>
            <div class="form-group">
              <label for="noMeja">Nomor Meja</label>
              <input
              type="text"
                v-model="pesan.noMeja"
                class="form-control"
                
              />
            </div>
            <button type="submit" class="btn btn-success float-right" @click="checkout">
              <b-icon-cart></b-icon-cart> Pesan
            </button>
          </form>
          </div>
  </div>

    </div>
  </div>
</template>

<script>
import Navbar from "../components/Navbar.vue";
import axios from "axios";
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Keranjang",
  components: {
    Navbar,
  },
  data() {
    return {
      keranjangs: [],
      pesan: {}
    };
  },
  methods: {
    setKeranjangs(data) {
      this.keranjangs = data;
    },

    deleteKeranjangs(id, nama_pesanan) {
      axios
        .delete("http://localhost:3000/keranjangs/" + id)
        .then(() => {
          this.$toast.error(`${nama_pesanan} berhasil dihapus `, {
            type: "error",
            position: "top-right",
            duration: 3000,
            dismissible: true,
          });
          axios
            .get("http://localhost:3000/keranjangs")
            .then((ResponseData) => {
              this.setKeranjangs(ResponseData.data);
            })
            .catch((e) => console.log(e));
        })
        .catch((e) => console.log(e));
    },
    checkout(){
      if(this.pesan.nama && this.pesan.noMeja){
        this.pesan.keranjangs = this.keranjangs
        
        axios.post("http://localhost:3000/pesanans", this.pesan)
        .then(() => {
          this.keranjangs.map((item) => {
            return axios.delete('http://localhost:3000/keranjangs/' + item.id)
            .catch(e => console.log(e))
          })
          this.$router.push({path: "/pesanan-sukses"})
        this.$toast.success(`sukses dipesan`, {
            type: "success",
            position: "top-right",
            duration: 3000,
            dismissible: true,
          });  
        })
        .catch(e => console.log(e))
      } else {
        this.$toast.error(`Nama dan Nomor meja harus diisi`, {
            type: "error",
            position: "top-right",
            duration: 3000,
            dismissible: true,
          });
      }
    }
  },
  mounted() {
    axios
      .get("http://localhost:3000/keranjangs")
      .then((ResponseData) => {
        this.setKeranjangs(ResponseData.data);
      })
      .catch((e) => console.log(e));
  },
  computed: {
    totalHarga() {
      return this.keranjangs.reduce((items, data) => {
        return items + data.products.harga * data.jumlah_pemesanan;
      }, 0);
    },
  },
};
</script>

<style>
</style>