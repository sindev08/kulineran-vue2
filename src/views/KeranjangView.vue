<template>
  <div class="keranjang">
    <NavBar :updateKeranjang="keranjang" />
    <div class="container">
      <div class="row mt-4">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods">Foods</router-link>
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
          <h2>Keranjang <strong>Saya</strong></h2>
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
                  :key="keranjang.id">
                  <th>{{ index + 1 }}</th>
                  <td>
                    <img
                      :src="'/assets/images/' + keranjang.products.gambar"
                      class="img-fluid"
                      width="250" />
                  </td>
                  <td>
                    <strong>{{ keranjang.products.nama }}</strong>
                  </td>
                  <td>
                    {{ keranjang.keterangan ? keranjang.keterangan : "-" }}
                  </td>
                  <td>
                    {{ keranjang.jumlah_pemesanan }}
                  </td>
                  <td align="center">{{ keranjang.products.harga }}</td>
                  <td align="center">
                    <strong>{{
                      keranjang.products.harga * keranjang.jumlah_pemesanan
                    }}</strong>
                  </td>
                  <td align="center" class="text-danger">
                    <b-icon-trash
                      @click="hapusKeranjang(keranjang.id)"></b-icon-trash>
                  </td>
                </tr>
                <tr>
                  <td colspan="6" align="right">
                    <strong>Total harga :</strong>
                  </td>
                  <td>
                    <strong>{{ totalHarga }}</strong>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>

      <div class="row justify-content-end">
        <div class="col-md-4">
          <form v-on:submit.prevent class="mt-4">
            <div class="form-group">
              <label for="nama">Nama</label>
              <input type="text" class="form-control" v-model="pesan.nama" />
            </div>
            <div class="form-group">
              <label for="noMeja">Nomor Meja</label>
              <textarea
                class="form-control"
                placeholder="Keterangan seperti : Pedas, Nasi Setengah"
                v-model="pesan.noMeja"></textarea>
            </div>
            <button
              type="submit"
              class="btn btn-success float-right"
              @click="checkout">
              <b-icon-cart></b-icon-cart> Pesan
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import NavBar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "KeranjangView",
  components: {
    NavBar,
  },
  data() {
    return {
      keranjangs: [],
      pesan: {},
    };
  },
  methods: {
    setKeranjangs(data) {
      this.keranjangs = data;
    },
    hapusKeranjang(id) {
      axios
        .delete("http://localhost:3000/keranjangs/" + id)
        .then(() => {
          this.$toast.success("Sukses hapus keranjang.", {
            // optional options Object
            type: "success",
            position: "top",
            duration: 3000,
            dismissible: true,
          });
        })
        .catch((error) => console.log("Gagal", error));

      // Update data keranjangs
      axios
        .get("http://localhost:3000/keranjangs")
        .then((response) => this.setKeranjangs(response.data))
        .catch((error) => console.log("Gagal", error));
    },
    checkout() {
      console.log(this.pesan);
      if (this.pesan.nama && this.pesan.noMeja) {
        this.pesan.keranjangs = this.keranjangs;
        axios
          .post("http://localhost:3000/pesanans", this.pesan)
          .then(() => {
            // Hapus semua keranjang
            this.keranjangs.map(function (item) {
              return axios
                .delete("http://localhost:3000/keranjangs/" + item.id)
                .catch((error) => console.log("Gagal", error));
            });

            this.$router.push({ path: "/pesanan-sukses" });
            this.$toast.success("Sukses Dipesan.", {
              // optional options Object
              type: "success",
              position: "top",
              duration: 3000,
              dismissible: true,
            });
          })
          .catch((error) => console.log("Gagal", error));
      } else {
        this.$toast.error("Nama dan Nomor Meja Harus diisi.", {
          // optional options Object
          type: "error",
          position: "top",
          duration: 3000,
          dismissible: true,
        });
      }
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/keranjangs")
      .then((response) => this.setKeranjangs(response.data))
      .catch((error) => console.log("Gagal", error));
  },
  computed: {
    totalHarga() {
      return this.keranjangs.reduce(function (items, data) {
        return items + data.products.harga * data.jumlah_pemesanan;
      }, 0);
    },
  },
};
</script>

<style></style>
