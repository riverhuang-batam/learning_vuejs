<template>
<b-navbar toggleable="lg" type="light" >
  <div class="container">
    <b-navbar-brand href="#">Kulineran</b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
         <li class="nav-item">
            <router-link class="nav-link" to="/">Home</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/Foods">Foods</router-link>
          </li>
      </b-navbar-nav>

      <!-- Right aligned nav items -->
      <b-navbar-nav class="ml-auto">
        <li class="nav-item">
            <router-link class="nav-link" to="/"
              >Keranjang
              <b-icon-bag></b-icon-bag>
              <span class="badge badge-success ml-2" >{{updateKeranjang ? updateKeranjang.length : this.jumlah_pesanan}}</span>
            </router-link>
          </li>
      </b-navbar-nav>
    </b-collapse>
    </div>
  </b-navbar>
</template>

<script>
import axios from "axios";
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Navbar",
  data() {
    return {
      jumlah_pesanan: 0,
    };
  },
  props: ['updateKeranjang'],
  methods: {
    setJumlahPesanan(data) {
      this.jumlah_pesanan = data;
    },
  },
  mounted() {
  axios.get('http://localhost:3000/keranjangs')
  .then(ResponseData => this.setJumlahPesanan(ResponseData.data.length))
  .catch(e => console.log(e));
  },
};

</script>

<style>
</style>