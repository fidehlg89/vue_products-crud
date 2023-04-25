<template>
  <div class="container">
    <div class="row">
      <div class="col-sm-10">
        <h1>Products</h1>
        <hr />
        <alert v-if="message" :message="message"></alert>
        <button
          type="button"
          class="btn btn-success btn-sm"
          @click="toggleAddProductModal"
        >
          <i class="bi bi-plus-circle"></i> Add New
        </button>
        <br /><br />
        <table class="table table-hover">
          <thead>
            <tr>
              <th scope="col">Name</th>
              <th scope="col">Description</th>
              <th scope="col">Price</th>
              <th scope="col">Quantity</th>
              <th scope="col">Available</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(product, index) in products" :key="index">
              <td>{{ product.name }}</td>
              <td>{{ product.desc }}</td>
              <td>{{ product.price }}</td>
              <td>{{ product.quantity }}</td>
              <td>
                <span v-if="product.available">Yes</span>
                <span v-else>No</span>
              </td>
              <td>
                <div class="btn-group" role="group">
                  <button
                    type="button"
                    class="btn btn-warning btn-sm"
                    @click="toggleEditProductModal(product)"
                  >
                    <i class="bi bi-pencil-square"></i>
                  </button>
                  <button
                    type="button"
                    class="btn btn-danger btn-sm"
                    @click="handleDeleteProduct(product)"
                  >
                    <i class="bi bi-trash"></i>
                  </button>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- add new product modal -->
    <div
      ref="addProductModal"
      class="modal fade"
      :class="{ show: activeAddProductModal, 'd-block': activeAddProductModal }"
      tabindex="-1"
      role="dialog"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-name">Add a new product</h5>
            <button
              type="button"
              class="close"
              data-dismiss="modal"
              aria-label="Close"
              @click="toggleAddProductModal"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <form>
              <div class="mb-3">
                <label for="addProductName" class="form-label">Name:</label>
                <input
                  type="text"
                  class="form-control"
                  id="addProductName"
                  v-model="addProductForm.name"
                  placeholder="Enter name"
                />
              </div>
              <div class="mb-3">
                <label for="addProductdesc" class="form-label"
                  >Description:</label
                >
                <input
                  type="text"
                  class="form-control"
                  id="addProductDesc"
                  v-model="addProductForm.desc"
                  placeholder="Enter description"
                />
              </div>
              <div class="mb-3">
                <label for="addProductPrice" class="form-label">Price:</label>
                <input
                  type="text"
                  class="form-control"
                  id="addProductPrice"
                  v-model="addProductForm.price"
                  placeholder="Enter price"
                />
              </div>
              <div class="mb-3">
                <label for="addProductQuantity" class="form-label"
                  >Quantity:</label
                >
                <input
                  type="text"
                  class="form-control"
                  id="addProductQuantity"
                  v-model="addProductForm.quantity"
                  placeholder="Enter quantity"
                />
              </div>
              <div class="mb-3 form-check">
                <input
                  type="checkbox"
                  class="form-check-input"
                  id="addProductAvailable"
                  v-model="addProductForm.available"
                />
                <label class="form-check-label" for="addProductAvailable"
                  >Available?</label
                >
              </div>
              <div class="btn-group" role="group">
                <button
                  type="button"
                  class="btn btn-primary btn-sm"
                  @click="handleAddSubmit"
                >
                  Submit
                </button>
                <button
                  type="button"
                  class="btn btn-danger btn-sm"
                  @click="handleAddReset"
                >
                  Reset
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
    <div v-if="activeAddProductModal" class="modal-backdrop fade show"></div>
    <!-- edit product modal -->
    <div
      ref="editProductModal"
      class="modal fade"
      :class="{
        show: activeEditProductModal,
        'd-block': activeEditProductModal,
      }"
      tabindex="-1"
      role="dialog"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-name">Update</h5>
            <button
              type="button"
              class="close"
              data-dismiss="modal"
              aria-label="Close"
              @click="toggleEditProductModal"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <form>
              <div class="mb-3">
                <label for="editProductName" class="form-label">Name:</label>
                <input
                  type="text"
                  class="form-control"
                  id="editProductTitle"
                  v-model="editProductForm.name"
                  placeholder="Enter name"
                />
              </div>
              <div class="mb-3">
                <label for="editProductDesc" class="form-label"
                  >Description:</label
                >
                <input
                  type="text"
                  class="form-control"
                  id="editProductDesc"
                  v-model="editProductForm.desc"
                  placeholder="Enter description"
                />
              </div>
              <div class="mb-3">
                <label for="editProductPrice" class="form-label"
                  >Price:</label
                >
                <input
                  type="text"
                  class="form-control"
                  id="editProductPrice"
                  v-model="editProductForm.price"
                  placeholder="Enter Price"
                />
              </div>
              <div class="mb-3">
                <label for="editProductQuantity" class="form-label"
                  >Quantity:</label
                >
                <input
                  type="text"
                  class="form-control"
                  id="editProductQuantity"
                  v-model="editProductForm.quantity"
                  placeholder="Enter quantity"
                />
              </div>
              <div class="mb-3 form-check">
                <input
                  type="checkbox"
                  class="form-check-input"
                  id="editProductAvailable"
                  v-model="editProductForm.available"
                />
                <label class="form-check-label" for="editProductAvailable"
                  >Available?</label
                >
              </div>
              <div class="btn-group" role="group">
                <button
                  type="button"
                  class="btn btn-primary btn-sm"
                  @click="handleEditSubmit"
                >
                  Submit
                </button>
                <button
                  type="button"
                  class="btn btn-danger btn-sm"
                  @click="handleEditCancel"
                >
                  Cancel
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
    <div v-if="activeEditProductModal" class="modal-backdrop fade show"></div>
  </div>
</template>

<script>
import axios from "axios";
import Alert from "./Alert.vue";

export default {
  data() {
    return {
      products: [],
      message: "",
      activeAddProductModal: false,
      addProductForm: {
        name: "",
        desc: "",
        available: [],
      },
      activeEditProductModal: false,
      editProductForm: {
        id: "",
        name: "",
        desc: "",
        available: [],
      },
    };
  },
  components: {
    alert: Alert,
  },
  methods: {
    addProduct(payload) {
      const path = "http://localhost:5001/products";
      axios
        .post(path, payload)
        .then(() => {
          this.getProducts();
        })
        .catch((error) => {
          console.log(error);
          this.getProducts();
        });
    },
    getProducts() {
      const path = "http://localhost:5001/products";
      axios
        .get(path)
        .then((res) => {
          this.products = res.data.products;
        })
        .catch((error) => {
          console.error(error);
        });
    },
    handleAddReset() {
      this.initForm();
    },
    handleAddSubmit() {
      this.toggleAddProductModal();
      let available = false;
      if (this.addProductForm.available[0]) {
        available = true;
      }
      const payload = {
        name: this.addProductForm.name,
        desc: this.addProductForm.desc,
        price: this.addProductForm.price,
        quantity: this.addProductForm.quantity,
        available, // property shorthand
      };
      this.addProduct(payload);
      this.initForm();
    },
    initForm() {
      this.addProductForm.name = "";
      this.addProductForm.desc = "";
      this.addProductForm.price = "";
      this.addProductForm.quantity = "";
      this.addProductForm.available = [];
      this.editProductForm.id = "";
      this.editProductForm.name = "";
      this.editProductForm.desc = "";
      this.editProductForm.price = "";
      this.editProductForm.quantity = "";
      this.editProductForm.available = [];
    },
    toggleAddProductModal() {
      const body = document.querySelector("body");
      this.activeAddProductModal = !this.activeAddProductModal;
      if (this.activeAddProductModal) {
        body.classList.add("modal-open");
      } else {
        body.classList.remove("modal-open");
      }
    },
    toggleEditProductModal(product) {
      if (product) {
        this.editProductForm = product;
      }
      const body = document.querySelector("body");
      this.activeEditProductModal = !this.activeEditProductModal;
      if (this.activeEditProductModal) {
        body.classList.add("modal-open");
      } else {
        body.classList.remove("modal-open");
      }
    },
    handleEditSubmit() {
      this.toggleEditProductModal(null);
      let available = false;
      if (this.editProductForm.available) available = true;
      const payload = {
        name: this.editProductForm.name,
        desc: this.editProductForm.desc,
        price: this.editProductForm.price,
        quantity: this.editProductForm.quantity,
        available,
      };
      this.updateProduct(payload, this.editProductForm.id);
    },
    updateProduct(payload, productID) {
      const path = `http://localhost:5001/products/${productID}`;
      axios
        .put(path, payload)
        .then(() => {
          this.getProducts();
          this.message = "Product updated!";
          this.showMessage = true;
        })
        .catch((error) => {
          console.error(error);
          this.getProducts();
        });
    },
    handleEditCancel() {
      this.toggleEditProductModal(null);
      this.initForm();
      this.getProducts(); // why?
    },
    handleDeleteProduct(product) {
      this.removeProduct(product.id);
    },
    removeProduct(productID) {
      const path = `http://localhost:5001/products/${productID}`;
      axios
        .delete(path)
        .then(() => {
          this.getProducts();
          this.message = "Product removed!";
          this.showMessage = true;
        })
        .catch((error) => {
          console.error(error);
          this.getProducts();
        });
    },
  },
  created() {
    this.getProducts();
  },
};
</script>
