<template>
  <div class="cart">
    <Navbar :updateCart="cart" />
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
          <!-- Empty Cart -->
          <div class="col-12 text-center mt-5" v-if="!cart.length">
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
                    <th scope="col">Image</th>
                    <th scope="col">Name</th>
                    <th scope="col">Description</th>
                    <th scope="col">Amount</th>
                    <th scope="col">Price</th>
                    <th scope="col">Total Price</th>
                    <th scope="col">Delete</th>
                  </tr>
                </thead>
                <tbody>
                  <tr
                    class="text-center"
                    v-for="(cart, index) in cart"
                    :key="cart.id"
                  >
                    <th class="text-center align-middle">{{ index + 1 }}</th>
                    <td class="align-middle">
                      <img
                        :src="'../assets/images/' + cart.products.image"
                        class="p-2"
                        width="200"
                      />
                    </td>
                    <td class="align-middle">
                      <strong>{{ cart.products.name }}</strong>
                    </td>
                    <td class="align-middle">
                      {{ cart.description ? cart.description : "-" }}
                    </td>
                    <td class="align-middle">
                      {{ cart.amount }}
                    </td>
                    <td class="align-middle" align="right">
                      Rp. {{ cart.products.price | numFormat("0,0") }}
                    </td>
                    <td class="align-middle" align="right">
                      <strong
                        >Rp.
                        {{
                          (cart.products.price *
                            cart.amount)
                            | numFormat("0,0")
                        }}</strong
                      >
                    </td>
                    <td align="center" class="text-danger align-middle">
                      <b-icon-trash
                        style="cursor: pointer"
                        font-scale="1.8"
                        @click="hapusCart(cart.id)"
                      ></b-icon-trash>
                    </td>
                  </tr>

                  <tr>
                    <td colspan="7" align="right">
                      <strong>Total Price :</strong>
                    </td>
                    <td align="right">
                      <strong>Rp. {{ totalPrice | numFormat("0,0") }}</strong>
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
                    <label for="name">Name :</label>
                    <input
                      type="text"
                      class="form-control"
                      v-model="pesan.name"
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
  name: "Cart",
  components: {
    Navbar,
  },
  data() {
    return {
      cart: [],
      pesan: {},
    };
  },
  methods: {
    setCarts(data) {
      this.cart = data;
    },
    hapusCart(id) {
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
              .delete("http://localhost:3000/cart/" + id)
              .then(() => {
                this.$swal.fire({
                  icon: "success",
                  title: "Deleted",
                  text: "Your food has been deleted from cart",
                });
                // Update Data cart
                axios
                  .get("http://localhost:3000/cart")
                  .then((response) => this.setCarts(response.data))
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
            if (this.pesan.name && this.pesan.noMeja) {
              this.pesan.cart = this.cart;
              axios
                .post("http://localhost:3000/orders", this.pesan)
                .then(() => {
                  // Hapus Semua Cart
                  this.cart.map(function (item) {
                    return axios
                      .delete("http://localhost:3000/cart/" + item.id)
                      .catch((error) => console.log(error));
                  });

                  this.$router.push({ path: "/order" });
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
      .get("http://localhost:3000/cart")
      .then((response) => this.setCarts(response.data))
      .catch((error) => console.log(error));
  },
  computed: {
    totalPrice() {
      return this.cart.reduce(function (items, data) {
        return items + data.products.price * data.amount;
      }, 0);
    },
  },
};
</script>