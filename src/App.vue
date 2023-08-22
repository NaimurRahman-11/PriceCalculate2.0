<template>
  <div id="app">
    <div class="container mt-5">
      <div class="row g-5 p-2">
        <!-- Left Column: Input -->
        <div class="col-md-8">
          <h2>Sell Your Goods</h2>
          <div class="form-group col-md-3">
            <label for="date" class="fw-bold">Date:</label>
            <input
              type="date"
              class="form-control form-control-sm"
              v-model="selectedDate"
            />
          </div>
          <br />

          <form id="inputForm">
            <div
              v-for="(item, index) in items"
              :key="index"
              class="form-row row gap-3 mb-4"
            >
              <div class="form-group col-md-4">
                <label for="goods" class="fw-bold">Select Your Goods:</label>
                <br />
                <select
                  class="form-select form-select-sm product-select"
                  v-model="item.selectedProduct"
                  aria-label="Large select example"
                >
                  <option value="">Select Your Goods</option>
                  <option value="Monitor">Monitor</option>
                  <option value="Laptop">Laptop</option>
                  <option value="SSD">SSD</option>
                  <option value="RAM">RAM</option>
                  <option value="Adapter">Adapter</option>
                  <option value="Camera">Camera</option>
                </select>
              </div>

              <div class="form-group col-md-2">
                <label class="fw-bold">Quantity:</label>
                <input
                  type="number"
                  class="form-control form-control-sm quantity-input"
                  v-model="item.quantity"
                  placeholder="Enter value"
                />
              </div>

              <div class="form-group col-md-2">
                <label class="fw-bold">Price (Per Unit):</label>
                <input
                  type="number"
                  class="form-control form-control-sm price-input"
                  v-model="item.price"
                  placeholder="Enter value"
                />
              </div>
            </div>

            <button type="button" class="btn btn-success" @click="addItem">
              + Add Another
            </button>
          </form>
        </div>

        <!-- Right Column: Output -->
        <div class="col-md-4 mb-5">
          <h2>Output</h2>
          <br />
          <form>
            <div class="">
              <div class="form-group col-md-8 mb-3">
                <label class="fw-bold">Total Price:</label>
                <input
                  type="text"
                  class="form-control form-control-sm"
                  v-model="totalPrice"
                  placeholder="Enter value"
                  readonly
                />
              </div>

              <div class="form-group col-md-8 mb-3">
                <label class="fw-bold">Paid:</label>
                <input
                  type="number"
                  class="form-control form-control-sm"
                  v-model="paidAmount"
                  placeholder="Enter value"
                />
              </div>

              <div class="form-group col-md-8 mb-3">
                <label class="fw-bold">Due:</label>
                <input
                  type="text"
                  class="form-control form-control-sm"
                  :value="dueAmount"
                  placeholder="Enter value"
                  readonly
                />
              </div>

              <button
                type="button"
                class="btn btn-success"
                @click="displaySummary"
              >
                Submit
              </button>
            </div>
          </form>
        </div>
      </div>

      <div
        id="summaryAlert"
        class="position-fixed bottom-0 end-0 mb-3 me-3"
        v-if="summaryVisible"
      >
        <div
          class="alert alert-success alert-dismissible fade show"
          role="alert"
        >
          <h4 class="alert-heading">Goods Summary</h4>
          <div v-for="(item, index) in items" :key="index">
            <p><strong>Product:</strong> {{ item.selectedProduct }}</p>
            <p><strong>Quantity:</strong> {{ item.quantity }}</p>
            <hr />
          </div>
          <p><strong>Total Price:</strong> BDT. {{ totalPrice }}</p>
          <p><strong>Paid Amount:</strong> BDT. {{ paidAmount }}</p>
          <p><strong>Due Amount:</strong> BDT. {{ dueAmount }}</p>
          <p><strong>Date:</strong> {{ selectedDate }}</p>
          <hr />
          <button type="button" class="btn btn-secondary" @click="closeSummary">
            Close
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      selectedDate: "",
      items: [
        {
          selectedProduct: "",
          quantity: "",
          price: "",
        },
      ],
      totalPrice: 0,
      paidAmount: 0,
      dueAmount: 0,
      summaryVisible: false,
    };
  },

  methods: {
    addItem() {
      this.items.push({
        selectedProduct: "",
        quantity: "",
        price: "",
      });
    },
    updateTotalPrice() {
      this.totalPrice = this.items.reduce((total, item) => {
        const quantity = parseFloat(item.quantity) || 0;
        const price = parseFloat(item.price) || 0;
        return total + Math.max(0, quantity) * Math.max(0, price);
      }, 0).toFixed(2);
      this.updateDueAmount();
    },
    updateDueAmount() {
      let paidAmount = parseFloat(this.paidAmount) || 0;
      paidAmount = Math.min(paidAmount, this.totalPrice);
      this.dueAmount = (this.totalPrice - paidAmount).toFixed(2);
    },

    displaySummary() {
      this.summaryVisible = true;
    },
    closeSummary() {
      this.summaryVisible = false;
    },
  },
  watch: {
    items: {
      handler: "updateTotalPrice",
      deep: true,
    },
    paidAmount: "updateDueAmount",
  },
};
</script>

<style>
</style>
