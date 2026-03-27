<template>
  <div class="products-view">
    <div class="header-section d-flex justify-content-between align-items-center mb-5">
      <div>
        <h1 class="mb-1">Inventory Management</h1>
        <p class="text-muted mb-0">Manage your product catalog with ease.</p>
      </div>
      <button
        type="button"
        class="btn btn-primary d-flex align-items-center gap-2 px-4 py-2 shadow-sm ripple"
        @click="toggleAddProductModal"
      >
        <i class="bi bi-plus-lg fs-5"></i>
        <span class="fw-semibold">Add Product</span>
      </button>
    </div>

    <!-- Alert / Toast -->
    <Alert :message="message" @clear="message = ''" />

    <!-- Products Table Card -->
    <div class="card border-0 shadow-sm overflow-hidden mb-4 product-card">
      <div class="table-responsive">
        <table class="table table-hover align-middle mb-0">
          <thead>
            <tr>
              <th scope="col" class="ps-4">Product Name</th>
              <th scope="col">Description</th>
              <th scope="col">Price</th>
              <th scope="col">Stock</th>
              <th scope="col">Status</th>
              <th scope="col" class="text-end pe-4">Actions</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(product, index) in products" :key="index" class="product-row">
              <td class="ps-4 fw-semibold text-dark">{{ product.name }}</td>
              <td class="text-muted truncate-text">{{ product.desc }}</td>
              <td class="fw-medium">${{ product.price }}</td>
              <td>
                <span class="badge rounded-pill" :class="product.quantity > 5 ? 'bg-light-success text-success' : 'bg-light-warning text-warning'">
                  {{ product.quantity }} units
                </span>
              </td>
              <td>
                <div class="d-flex align-items-center gap-2">
                  <span class="status-indicator" :class="product.available ? 'online' : 'offline'"></span>
                  <span class="fw-medium fs-7">{{ product.available ? 'Available' : 'Out of Stock' }}</span>
                </div>
              </td>
              <td class="text-end pe-4">
                <div class="action-buttons d-flex justify-content-end gap-2">
                  <button
                    type="button"
                    class="btn btn-icon btn-light-warning"
                    title="Edit Product"
                    @click="toggleEditProductModal(product)"
                  >
                    <i class="bi bi-pencil-fill"></i>
                  </button>
                  <button
                    type="button"
                    class="btn btn-icon btn-light-danger"
                    title="Delete Product"
                    @click="handleDeleteProduct(product)"
                  >
                    <i class="bi bi-trash3-fill"></i>
                  </button>
                </div>
              </td>
            </tr>
            <tr v-if="products.length === 0">
              <td colspan="6" class="text-center py-5 text-muted fst-italic">
                No products found. Start by adding one!
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- Modals -->
    <!-- Add Product Modal -->
    <div
      class="modal fade custom-modal"
      :class="{ show: activeAddProductModal, 'd-block': activeAddProductModal }"
      tabindex="-1"
    >
      <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content border-0 shadow-lg">
          <div class="modal-header border-0 pb-0">
            <h5 class="modal-title fw-bold fs-4">New Product</h5>
            <button type="button" class="btn-close" @click="toggleAddProductModal"></button>
          </div>
          <div class="modal-body p-4">
            <form @submit.prevent="handleAddSubmit">
              <div class="mb-3">
                <label class="form-label fw-semibold">Name</label>
                <input v-model="addProductForm.name" type="text" class="form-control custom-input" placeholder="Product name" required />
              </div>
              <div class="mb-3">
                <label class="form-label fw-semibold">Description</label>
                <textarea v-model="addProductForm.desc" class="form-control custom-input" rows="3" placeholder="Tell us about the product"></textarea>
              </div>
              <div class="row mb-3">
                <div class="col">
                  <label class="form-label fw-semibold">Price</label>
                  <div class="input-group">
                    <span class="input-group-text bg-white border-end-0">$</span>
                    <input v-model.number="addProductForm.price" type="number" step="0.01" class="form-control custom-input border-start-0" required />
                  </div>
                </div>
                <div class="col">
                  <label class="form-label fw-semibold">Quantity</label>
                  <input v-model.number="addProductForm.quantity" type="number" class="form-control custom-input" required />
                </div>
              </div>
              <div class="mb-4 d-flex align-items-center gap-3">
                 <div class="form-check form-switch custom-switch">
                  <input v-model="addProductForm.available" class="form-check-input" type="checkbox" id="addAvailable">
                  <label class="form-check-label fw-medium" for="addAvailable">Available for sale</label>
                </div>
              </div>
              <div class="modal-footer border-0 p-0 d-flex gap-2">
                <button type="button" class="btn btn-light-secondary flex-grow-1 py-2" @click="toggleAddProductModal">Cancel</button>
                <button type="submit" class="btn btn-primary flex-grow-1 py-2 shadow-sm">Create Product</button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>

    <!-- Edit Product Modal -->
    <div
      class="modal fade custom-modal"
      :class="{ show: activeEditProductModal, 'd-block': activeEditProductModal }"
      tabindex="-1"
    >
      <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content border-0 shadow-lg">
          <div class="modal-header border-0 pb-0">
            <h5 class="modal-title fw-bold fs-4">Edit Product</h5>
            <button type="button" class="btn-close" @click="toggleEditProductModal(null)"></button>
          </div>
          <div class="modal-body p-4">
            <form @submit.prevent="handleEditSubmit">
              <div class="mb-3">
                <label class="form-label fw-semibold">Name</label>
                <input v-model="editProductForm.name" type="text" class="form-control custom-input" required />
              </div>
              <div class="mb-3">
                <label class="form-label fw-semibold">Description</label>
                <textarea v-model="editProductForm.desc" class="form-control custom-input" rows="3"></textarea>
              </div>
               <div class="row mb-3">
                <div class="col">
                  <label class="form-label fw-semibold">Price</label>
                   <div class="input-group">
                    <span class="input-group-text bg-white border-end-0">$</span>
                    <input v-model.number="editProductForm.price" type="number" step="0.01" class="form-control custom-input border-start-0" required />
                  </div>
                </div>
                <div class="col">
                  <label class="form-label fw-semibold">Quantity</label>
                  <input v-model.number="editProductForm.quantity" type="number" class="form-control custom-input" required />
                </div>
              </div>
              <div class="mb-4 d-flex align-items-center gap-3">
                 <div class="form-check form-switch custom-switch">
                  <input v-model="editProductForm.available" class="form-check-input" type="checkbox" id="editAvailable">
                  <label class="form-check-label fw-medium" for="editAvailable">Available for sale</label>
                </div>
              </div>
              <div class="modal-footer border-0 p-0 d-flex gap-2">
                <button type="button" class="btn btn-light-secondary flex-grow-1 py-2" @click="toggleEditProductModal(null)">Cancel</button>
                <button type="submit" class="btn btn-primary flex-grow-1 py-2 shadow-sm">Update Product</button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>

    <!-- Backdrop -->
    <div v-if="activeAddProductModal || activeEditProductModal" class="modal-backdrop fade show overlay-blur"></div>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted } from "vue";
import axios from "axios";
import Alert from "./Alert.vue";

// State
const products = ref([]);
const message = ref("");
const activeAddProductModal = ref(false);
const activeEditProductModal = ref(false);

const addProductForm = reactive({
  name: "",
  desc: "",
  price: 0,
  quantity: 0,
  available: false,
});

const editProductForm = reactive({
  id: "",
  name: "",
  desc: "",
  price: 0,
  quantity: 0,
  available: false,
});

// Methods
const getProducts = async () => {
  const path = "http://localhost:5001/products";
  try {
    const res = await axios.get(path);
    products.value = res.data.products;
  } catch (error) {
    console.error("Error fetching products:", error);
  }
};

const addProduct = async (payload) => {
  const path = "http://localhost:5001/products";
  try {
    await axios.post(path, payload);
    message.value = "Product added successfully!";
    getProducts();
  } catch (error) {
    console.error("Error adding product:", error);
    getProducts();
  }
};

const updateProduct = async (payload, productID) => {
  const path = `http://localhost:5001/products/${productID}`;
  try {
    await axios.put(path, payload);
    message.value = "Product updated successfully!";
    getProducts();
  } catch (error) {
    console.error("Error updating product:", error);
    getProducts();
  }
};

const removeProduct = async (productID) => {
  const path = `http://localhost:5001/products/${productID}`;
  try {
    await axios.delete(path);
    message.value = "Product removed!";
    getProducts();
  } catch (error) {
    console.error("Error deleting product:", error);
    getProducts();
  }
};

const toggleAddProductModal = () => {
  activeAddProductModal.value = !activeAddProductModal.value;
  if (!activeAddProductModal.value) initForm();
};

const toggleEditProductModal = (product) => {
  if (product) {
    editProductForm.id = product.id;
    editProductForm.name = product.name;
    editProductForm.desc = product.desc;
    editProductForm.price = product.price;
    editProductForm.quantity = product.quantity;
    editProductForm.available = !!product.available;
  }
  activeEditProductModal.value = !activeEditProductModal.value;
};

const handleAddSubmit = () => {
  const payload = { ...addProductForm };
  addProduct(payload);
  toggleAddProductModal();
};

const handleEditSubmit = () => {
  const { id, ...payload } = editProductForm;
  updateProduct(payload, id);
  toggleEditProductModal(null);
};

const handleDeleteProduct = (product) => {
  if (confirm(`Are you sure you want to delete ${product.name}?`)) {
    removeProduct(product.id);
  }
};

const initForm = () => {
  addProductForm.name = "";
  addProductForm.desc = "";
  addProductForm.price = 0;
  addProductForm.quantity = 0;
  addProductForm.available = false;
};

onMounted(() => {
  getProducts();
});
</script>

<style scoped>
.products-view {
  animation: fadeIn 0.6s ease-out;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

/* Typography & Layout */
h1 { font-size: 2.25rem; font-weight: 800; color: #0f172a; }
.truncate-text { max-width: 250px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }

/* Product Card & Table */
.product-card {
  background: var(--surface);
  border-radius: var(--border-radius);
  border: 1px solid rgba(226, 232, 240, 0.8) !important;
}

.table thead th {
  background: #f8fafc;
  color: var(--text-muted);
  font-weight: 600;
  font-size: 0.85rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  padding: 1.25rem 1rem;
  border-bottom: 1px solid #e2e8f0;
}

.table tbody tr {
  transition: background 0.2s ease;
}

.table tbody tr:hover {
  background-color: #f1f5f9;
}

/* Badges & Indicators */
.bg-light-success { background: #d1fae5; color: #065f46; }
.bg-light-warning { background: #ffedd5; color: #9a3412; }
.bg-light-danger { background: #fee2e2; color: #991b1b; }

.status-indicator {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  display: inline-block;
}
.status-indicator.online { background: #10b981; box-shadow: 0 0 0 3px rgba(16, 185, 129, 0.2); }
.status-indicator.offline { background: #94a3b8; }

/* Buttons */
.btn-primary {
  background: var(--primary);
  border: none;
  border-radius: 10px;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}
.btn-primary:hover {
  background: var(--primary-hover);
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(99, 102, 241, 0.25);
}

.btn-icon {
  width: 32px;
  height: 32px;
  padding: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 8px;
  transition: all 0.2s ease;
}

.btn-light-warning { background: #fffbeb; color: #d97706; border: none; }
.btn-light-warning:hover { background: #fef3c7; color: #b45309; }

.btn-light-danger { background: #fef2f2; color: #dc2626; border: none; }
.btn-light-danger:hover { background: #fee2e2; color: #b91c1c; }

.btn-light-secondary { background: #f1f5f9; color: #475569; border: none; font-weight: 500; }
.btn-light-secondary:hover { background: #e2e8f0; }

/* Modals & Inputs */
.overlay-blur { backdrop-filter: blur(4px); background: rgba(15, 23, 42, 0.4); }

.custom-modal :deep(.modal-content) {
  border-radius: 20px;
}

.custom-input {
  border: 1px solid #e2e8f0;
  border-radius: 10px;
  padding: 0.625rem 0.875rem;
  transition: all 0.2s ease;
}

.custom-input:focus {
  border-color: var(--primary);
  box-shadow: 0 0 0 4px rgba(99, 102, 241, 0.1);
}

/* Switch Styling */
.custom-switch .form-check-input {
  width: 2.8rem;
  height: 1.5rem;
  cursor: pointer;
}
.custom-switch .form-check-input:checked {
  background-color: var(--primary);
  border-color: var(--primary);
}

.fs-7 { font-size: 0.875rem; }
</style>
