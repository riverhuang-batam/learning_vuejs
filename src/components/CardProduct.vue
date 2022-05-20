<template>
<div class="card shadow card-product" style="width: 18rem;">
  <img v-if="product.gambar"
            :src="require(`@/assets/images/${product.gambar}`)" class="card-img-top" width="100%">
  
  
  <div class="card-body">
    <h5 class="card-title">{{product.nama}}</h5>
    <p class="card-text">Harga : Rp. {{product.harga}}</p>
    <router-link :to="'foods/' + product.id" class="btn btn-success"><b-icon-cart></b-icon-cart>Pesan</router-link>
  </div>
</div>
  <!-- <h2>
      {{product.nama}}
  </h2> -->
</template>

<script>
export default {
    name: 'CardProduct',
    props: {
        product: Object
    }
}
</script>

<style>

</style>