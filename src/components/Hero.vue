<template>
  
  <div class="row flex-md-row flex-column-reverse mt-4">
      <div class="col-md-6 mb-3">
          <div class="d-flex h-100">
          <div class="justify-content-center align-self-center">
          <h2><strong>Delicious Food Menu. </strong> <br> in Your Gadget</h2>
          <p>Ayo segera pilih dan pesan makanan favorit Anda</p>
          <router-link to="/foods" class="btn btn-lg btn-success"><b-icon-arrow-right></b-icon-arrow-right> Pesan</router-link>
          </div>
          </div>
      </div>
      <div class="col-md-6 mb-3 `">
          <img src="../assets/images/eating_together.png" width="100%">
      </div>
  </div>
</template>

<script>
export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name: 'hero'
}
</script>

<style>

</style>