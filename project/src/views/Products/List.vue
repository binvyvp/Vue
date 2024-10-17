<template>
  <div class="products-list">
    <div class="pricing-header px-3 py-3 pt-md-5 pb-md-4 mx-auto text-center">
      <h2 class="display-4">Product Management</h2>
      <router-link to="/product/create">Add New</router-link>
    </div>

    <div class="container">
      <div class="card-deck mb-3">
        <table class="table table-bordered">
          <thead>
            <tr>
              <th scope="col">STT</th>
              <th scope="col">Product name</th>
              <th scope="col">Price</th>
              <th scope="col">Handle</th>
            </tr>
          </thead>
          <tbody>
            <tr :key="index" v-for="(product, index) in products">
              <th scope="row">{{ product.id }}</th>
              <td>{{ product.name }}</td>
              <td>{{ product.price }}</td>
              <td>
                <router-link :to="{ name: 'product.edit', params: { id: product.id } }"
                  ><button class="btn btn-primary">Sửa</button>
                </router-link>
                &nbsp;
                <button class="btn btn-danger" @click="onDelete(product.id)">Xóa</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductList",
  data() {
    return {
      products: [],
    };
  },
  created() {
    this.getAll();
  },
  methods: {
    getAll() {
      this.$request.get("http://localhost:8000/api/products").then((res) => {
        this.products = res.data;
      });
    },
    onDelete(productId) {
      this.$swal
        .fire({
          title: "Bạn muốn xóa?",
          showDenyButton: false,
          showCancelButton: true,
          confirmButtonText: "Xóa",
        })
        .then((result) => {
          /* Read more about isConfirmed, isDenied below */
          if (result.isConfirmed) {
            this.$request
              .delete(`http://localhost:8000/api/products/${productId}`)
              .then((res) => {
                if (res.data.success) {
                  this.$swal.fire("Delete!", "", "success");
                  this.getAll();
                }
              });
          } else if (result.isDenied) {
            Swal.fire("Changes are not saved", "", "info");
          }
        });
    },
  },
};
</script>
