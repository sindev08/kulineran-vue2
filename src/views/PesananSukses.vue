<template>
  <div class="pesanan-sukses">
    <NavBar />
    <div class="container">
      <div class="row justify-content-center">
        <div class="col text-center">
          <img src="../assets/images/menunggu.png" width="300" />
          <h2><strong>Sukses !</strong></h2>
          <h4>Pesanan Anda segera diproses <br />Selamat Menunggu</h4>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import NavBar from "@/components/Navbar.vue";

export default {
  name: "PesananSukses",
  components: {
    NavBar,
  },
};
</script>

<style></style>
