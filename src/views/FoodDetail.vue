<template>
  <div>
    <Navbar />
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
                Food Detail
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row mt-3">
        <div class="col-md-6">
          <img
            v-if="product.gambar"
            :src="require(`@/assets/images/${product.gambar}`)"
            class="img-fluid shadow"
          />
        </div>
        <div class="col-md-6">
          <h2>
            <strong>{{ product.nama }}</strong>
          </h2>
          <h4>
            Harga : <strong>Rp.{{ product.harga }}</strong>
          </h4>
          <form action="" class="mt-4" v-on:submit.prevent>
            <div class="form-group">
              <label for="Jumlah Pemesanan">Jumlah Pesan</label>
              <input
                type="number"
                class="form-control"
                v-model="pesan.jumlah_pemesanan"
              />
            </div>
            <div class="form-group">
              <label for="Keterangan">Keterangan</label>
              <textarea
                v-model="pesan.keterangan"
                class="form-control"
                placeholder="Keterangan spt: Pedes, Nasi Setengah"
              ></textarea>
            </div>
            <button type="submit" class="btn btn-success" @click="pemesanan">
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
  name: "FoodDetail",
  components: {
    Navbar,
  },
  data() {
    return {
      product: {},
      pesan: {},
    };
  },
  methods: {
    setProduct(data) {
      this.product = data;
    },

    pemesanan() {
      this.pesan.products = this.product;
      if (this.pesan.jumlah_pemesanan) {
        axios
          .post("http://localhost:3000/keranjangs/", this.pesan)
          .then(() =>{
          this.$router.push({path: "/keranjang"})
            this.$toast.success("Berhasil masuk ke keranjang", {
              type: "success",
              position: "top-right",
              duration: 3000,
              dismissible: true,
            })
          })
          .catch((e) => console.log(e));
      } else {
        this.$toast.error("Jumlah pesanan harus diisi", {
          type: "error",
          position: "top-right",
          duration: 3000,
          dismissible: true,
        });
      }
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/products/" + this.$route.params.id)
      .then((ResponseData) => this.setProduct(ResponseData.data))
      .then(() => console.log(this.product))
      .catch((e) => console.log(e));
  },
};
</script>

<style>
</style>