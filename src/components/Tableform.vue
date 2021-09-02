<template>
  <div class="container mt-10">
    <table class="table table-bordered mt-5">
      <thead class="thead-light">
        <tr>
          <th width="5%">#</th>
          <th width="30%">Номеклатура</th>
          <th width="10%">Количество</th>
          <th width="10%">Цена</th>
          <th width="10%">НДС</th>
          <th width="15%">Сумма</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in items" :key="index">
          <td>{{ index + 1 }}</td>
          <td>
            <span v-if="editIndex !== index">{{ item.nomenclature }}</span>
            <span v-if="editIndex === index">
              <input
                class="form-control form-control-sm"
                v-model="item.nomenclature"
              />
            </span>
          </td>
          <td>
            <span v-if="editIndex !== index">{{ item.quantity }} </span>
            <span v-if="editIndex === index">
              <input
                class="form-control form-control-sm"
                type="number"
                v-model="item.quantity"
                placeholder="0"
              />
            </span>
          </td>
          <td>
            <span v-if="editIndex !== index">{{ item.price }}</span>
            <span v-if="editIndex === index">
              <input
                class="form-control form-control-sm"
                type="number"
                v-model="item.price"
                placeholder="0"
              />
            </span>
          </td>

          <td>
            <div class="text-right">{{ subtotalNdc(item) }}</div>
          </td>
          <td>
            <div class="text-right">{{ subtotal(item) }}</div>
          </td>
          <td>
            <span v-if="editIndex !== index">
              <button
                @click="edit(item, index)"
                class="btn btn-sm btn-outline-secondary mr-2"
              >
                Редактировать
              </button>
              <button
                @click="remove(item, index)"
                class="btn btn-sm btn-outline-secondary mr-2"
              >
                Удалить
              </button>
            </span>
            <span v-else>
              <button
                class="btn btn-sm btn-outline-secondary mr-2"
                @click="cancel(item)"
              >
                Назад
              </button>
              <button
                class="btn btn-sm btn-outline-secondary mr-2"
                @click="
                  save(item);
                  exportItems();
                "
              >
                Сохранить
              </button>
            </span>
          </td>
        </tr>
      </tbody>
    </table>

    <div class="col-3 offset-9 text-right my-3">
      <button v-show="!editIndex" @click="add" class="btn btn-sm btn-secondary">
        Добавить товар
      </button>
    </div>

    <div class="col-3 offset-9">
      <div class="input-group">
        <div class="input-group-prepend">
          <span class="input-group-text">НДС</span>
        </div>
        <input class="form-control" disabled :value="this.allSubTotalNdc" />
      </div>
      <div class="input-group">
        <div class="input-group-prepend">
          <span class="input-group-text">Всего</span>
        </div>
        <input class="form-control" disabled :value="this.allSubTotal" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    selectNdc: {
      type: String,
      default: "",
    },
  },

  data() {
    return {
      editIndex: null,
      originalData: null,
      items: [],
    };
  },

  methods: {
    add() {
      this.originalData = null;
      this.items.push({
        nomenclature: "",
        quantity: 0,
        price: 0,
        amount: 0,
        ndc: 0,
      });
      this.editIndex = this.items.length - 1;
    },

    edit(item, index) {
      this.originalData = Object.assign({}, item);
      this.editIndex = index;
    },

    cancel(item) {
      this.editIndex = null;

      if (!this.originalData) {
        this.items.splice(this.items.indexOf(item), 1);
        return;
      }

      Object.assign(item, this.originalData);
      this.originalData = null;
    },

    remove(index) {
      this.items.splice(index, 1);
    },

    save() {
      this.originalData = null;
      this.editIndex = null;
    },

    subtotal(item) {
      return (item.amount = item.quantity * item.price);
    },

    subtotalNdc(item) {
      if (this.selectNdc === "Без ндс") {
        return 0;
      }
      item.ndc = (item.quantity * item.price * parseInt(this.selectNdc)) / 120;
      return Number(item.ndc.toFixed(2));
    },
    exportItems() {
      this.$emit("itemsarray", this.items);
    },
  },

  computed: {
    allSubTotal() {
      return this.items
        .map((item) => this.subtotal(item))
        .reduce((a, b) => a + b, 0);
    },

    allSubTotalNdc() {
      return this.items
        .map((item) => this.subtotalNdc(item))
        .reduce((a, b) => a + b, 0);
    },
  },
};
</script>

<style>
.table {
  width: 100%;
}
</style>
