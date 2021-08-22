<template>
  <div class="container mt-4">

    <table class="table table-bordered mt-4">
      <thead class="thead-light">
        <tr>
          <th width="1">#</th>
          <th width="10%">nomenclature</th>
          <th width="10%">quantity</th>
          <th width="10%">price</th>
          <th width="10%">amount</th>
          <th width="10%">ndc</th>
          <th width="10%">Total</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in items" :key="index">
          <td>{{ index + 1 }}</td>
          <td>
            <span v-if="editIndex !== index">{{ item.nomenclature }}</span>
            <span v-if="editIndex === index">
              <input class="form-control form-control-sm" v-model="item.nomenclature">
            </span>
          </td>
          <td>
            <span v-if="editIndex !== index">{{ item.quantity }}</span>
            <span v-if="editIndex === index">
              <input class="form-control form-control-sm" v-model="item.quantity">
            </span>
          </td>
          <td>
            <span v-if="editIndex !== index">{{ item.price }}</span>
            <span v-if="editIndex === index">
              <input class="form-control form-control-sm" v-model="item.price">
            </span>
          </td>
          <td>
            <span v-if="editIndex !== index">{{ item.amount }}</span>
            <span v-if="editIndex === index">
              <input class="form-control form-control-sm" type="number" v-model.number="item.amount">
            </span>
          </td>
          <td>
            <span v-if="editIndex !== index">{{ item.ndc }}</span>
            <span v-if="editIndex === index">
              <input class="form-control form-control-sm" v-model="item.ndc">
            </span>
          </td>
          <td><div class="text-right">{{ subtotal(item)  }}</div></td>
          <td>
            <span v-if="editIndex !== index">
              <button @click="edit(item, index)" class="btn btn-sm btn-outline-secondary mr-2">Edit</button>
              <button @click="remove(item, index)" class="btn btn-sm btn-outline-secondary mr-2">Remove</button>
            </span>
            <span v-else>
              <button class="btn btn-sm btn-outline-secondary mr-2" @click="cancel(item)">Cancel</button>
              <button class="btn btn-sm btn-outline-secondary mr-2" @click="save(item)">Save</button>
            </span>
          </td>
        </tr>
      </tbody>
    </table>

    <div class="col-3 offset-9 text-right my-3">
      <button v-show="!editIndex" @click="add" class="btn btn-sm btn-secondary">Add item</button>
    </div>

    <div class="col-3 offset-9">

      <div class="input-group input-group-lg mb-3">
        <div class="input-group-prepend">
          <span class="input-group-text">Total</span>
        </div>
        <input class="form-control form-control-sm text-right" disabled :value="this.total ">
      </div>

    </div>

  </div>
</template>

<script>
export default {

  

  data() {
    return {
      editIndex: null,
      originalData: null,
      items: [],
      tax: 0,
    }
  },

  methods: {

    add() {
      this.originalData = null
      this.items.push({ nomenclature: '', quantity: 0, price: 0, amount: 0, ndc: 0 })
      this.editIndex = this.items.length - 1
    },

    edit(item, index) {
      this.originalData = Object.assign({}, item)
      this.editIndex = index
    },

    cancel(item) {
      this.editIndex = null

      if (!this.originalData) {
        this.items.splice(this.items.indexOf(item), 1)
        return
      }

      Object.assign(item, this.originalData)
      this.originalData = null
    },

    remove( index) {
      this.items.splice(index, 1)
    },

    save() {
      this.originalData = null
      this.editIndex = null
    },

    subtotal(item) {
      return item.amount * item.price
    }

  },

  computed: {

    allSubTotal() {
      return this.items
        .map(item => this.subtotal(item))
        .reduce((a, b) => a + b, 0)
    },

    total() {
      return this.tax
        ? this.allSubTotal + (this.allSubTotal * (this.tax / 100))
        : this.allSubTotal
    }

  },

}
</script>

<style>
  input[type="number"] {
    text-align: center;
  }
</style>
