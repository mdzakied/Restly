<template>
  <div class="keranjang">
    <Navbar :updateKeranjang="keranjangs" />
    <div class="container">
      <!-- Breadcrumb -->
      <div class="row mt-4">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/" class="text-light">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods" class="text-light">Foods</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">
                Cart Food
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row mt-4">
        <div class="col">
          <h2>
            My
            <strong>Cart Food</strong>
          </h2>
          <!-- Empety Cart -->
          <div class="col-12 text-center mt-5" v-if="!keranjangs.length">
            <img
              src="../assets/images/empty-cart.png"
              alt="image empty food"
              style="max-width: 250px"
            />
            <h2 class="mt-4"><strong>Sory :(</strong></h2>
            <h4>
              Your Cart is Empty<br />
              Add Food You Want to Order
            </h4>
          </div>
          <!-- Cart -->
          <div v-else>
            <div class="table-responsive mt-5">
              <table class="table text-light">
                <thead>
                  <tr class="text-center">
                    <th scope="col">No</th>
                    <th scope="col">Food</th>
                    <th scope="col">Name Food</th>
                    <th scope="col">Description</th>
                    <th scope="col">Item Food</th>
                    <th scope="col">Price</th>
                    <th scope="col">Total Price</th>
                    <th scope="col">Delete</th>
                  </tr>
                </thead>
                <tbody>
                  <tr
                    class="text-center"
                    v-for="(keranjang, index) in keranjangs"
                    :key="keranjang.id"
                  >
                    <th class="text-center align-middle">{{ index + 1 }}</th>
                    <td class="align-middle">
                      <img
                        :src="'../assets/images/' + keranjang.products.gambar"
                        class="p-2"
                        width="200"
                      />
                    </td>
                    <td class="align-middle">
                      <strong>{{ keranjang.products.nama }}</strong>
                    </td>
                    <td class="align-middle">
                      {{ keranjang.keterangan ? keranjang.keterangan : "-" }}
                    </td>
                    <td class="align-middle">
                      {{ keranjang.jumlah_pemesanan }}
                    </td>
                    <td class="align-middle" align="right">
                      Rp. {{ keranjang.products.harga | numFormat("0,0") }}
                    </td>
                    <td class="align-middle" align="right">
                      <strong
                        >Rp.
                        {{
                          (keranjang.products.harga *
                            keranjang.jumlah_pemesanan)
                            | numFormat("0,0")
                        }}</strong
                      >
                    </td>
                    <td align="center" class="text-danger align-middle">
                      <b-icon-trash
                        style="cursor: pointer"
                        font-scale="1.8"
                        @click="hapusKeranjang(keranjang.id)"
                      ></b-icon-trash>
                    </td>
                  </tr>

                  <tr>
                    <td colspan="7" align="right">
                      <strong>Total Harga :</strong>
                    </td>
                    <td align="right">
                      <strong>Rp. {{ totalHarga | numFormat("0,0") }}</strong>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>

            <!-- Form Checkout -->
            <div class="row justify-content-end">
              <div class="col-md-4">
                <form class="mt-4" v-on:submit.prevent>
                  <div class="form-group">
                    <label for="nama">Name :</label>
                    <input
                      type="text"
                      class="form-control"
                      v-model="pesan.nama"
                      placeholder="Mr. Yondi"
                    />
                  </div>
                  <div class="form-group">
                    <label for="noMeja">No. Table :</label>
                    <input
                      type="text"
                      class="form-control"
                      v-model="pesan.noMeja"
                      placeholder="31-B-01"
                    />
                  </div>

                  <button
                    type="submit"
                    class="btn btn-success float-right"
                    @click="checkout"
                  >
                    Order
                  </button>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "Keranjang",
  components: {
    Navbar,
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
      this.$swal
        .fire({
          title: "Are you sure ?",
          text: "You delete this food from cart",
          icon: "warning",
          showCancelButton: true,
          confirmButtonColor: "#3085d6",
          cancelButtonColor: "#d33",
          confirmButtonText: "Yes, delete it !",
        })
        .then((result) => {
          if (result.isConfirmed) {
            axios
              .delete("http://localhost:3000/keranjangs/" + id)
              .then(() => {
                this.$swal.fire({
                  icon: "error",
                  title: "Deleted",
                  text: "Your food has been deleted from cart",
                });
                // Update Data keranjang
                axios
                  .get("http://localhost:3000/keranjangs")
                  .then((response) => this.setKeranjangs(response.data))
                  .catch((error) => console.log(error));
              })
              .catch((error) => console.log(error));
          }
        });
    },
    checkout() {
      this.$swal
        .fire({
          title: "Are you sure ?",
          text: "You won't be able to revert this !",
          icon: "warning",
          showCancelButton: true,
          confirmButtonColor: "#3085d6",
          cancelButtonColor: "#d33",
          confirmButtonText: "Yes, order it !",
        })
        .then((result) => {
          if (result.isConfirmed) {
            if (this.pesan.nama && this.pesan.noMeja) {
              this.pesan.keranjangs = this.keranjangs;
              axios
                .post("http://localhost:3000/pesanans", this.pesan)
                .then(() => {
                  // Hapus Semua Keranjang
                  this.keranjangs.map(function (item) {
                    return axios
                      .delete("http://localhost:3000/keranjangs/" + item.id)
                      .catch((error) => console.log(error));
                  });

                  this.$router.push({ path: "/pesanan-sukses" });
                  this.$swal.fire({
                    icon: "success",
                    title: "Success",
                    text: "Your order has been received",
                  });
                })
                .catch((err) => console.log(err));
            } else {
              this.$swal.fire({
                icon: "question",
                title: "Oops...",
                text: "Your name and number table is not filled ",
              });
            }
          }
        });
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/keranjangs")
      .then((response) => this.setKeranjangs(response.data))
      .catch((error) => console.log(error));
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