<template>
  <div class="form-label-margin-bottom">
    <select v-model="select" @click="selectItem">
      <option disabled value="">Выберите ораганизацию</option>
      <option v-for="listOoo in counterparty" :key="listOoo.fields.name">
        {{ listOoo.fields.name }}
      </option>
    </select>
    <button @click="watchModal()" class="btn btn-sm">Посмотреть</button>
    <button @click="change()" class="btn btn-sm">Изменить</button>
    <button @click="deleteOrg()" class="btn btn-sm">Удалить</button>

    <ModalWindow v-if="showModal" @close="showModal = false">
        <FormInput slot="body"  
        @close="showModal = false"
        :fillingCounterparty="counterpartyActiv"
        :selectVisible="selectVisible"
        :buttonVisible="buttonVisible"
        :disabledInput="disabledInput"
        />
        <h2 slot="header">Данные Организации</h2>
      </ModalWindow>
  </div>
</template>

<script>
import FormInput from "@/components/FormInput.vue";
import ModalWindow from "@/components/ModalWindow.vue";
import { bus } from "../main";

export default {
  props: {
    counterparty: {
      type: Array,
      default: () => [],
    },
  },

  data() {
    return {
      disabledInput: false,
      showModal: false,
      select: "",
      counterpartyActiv: {},
      selectVisible: true,
      buttonVisible: true,
    };
  },
  components: {
    ModalWindow,
    FormInput
  },
  methods: {
    getSelectOrganization() {
      if (this.counterparty !== null)
        this.counterparty.forEach((element) => {
          if (element.fields.name === this.select) {
            return (this.counterpartyActiv = element);
          }
        });
    },

    watchModal() {
      this.showModal = true;
      this.getSelectOrganization();
      this.selectVisible = false;
      this.buttonVisible = false;
      this.disabledInput = true;
    },

    change() {
      this.showModal = true;
      this.getSelectOrganization();
      this.selectVisible = true;
      this.buttonVisible = true;
      this.disabledInput = false;
    },

    deleteOrg() {
      let listOrgCount = this.counterparty.listOrgCount;
      const idx = this.counterparty.findIndex((i) => i.fields.name === this.select);
      console.log(idx)
      if (idx >= 0) {
        this.counterparty.splice(idx, 1);
      }
      if (listOrgCount === "Counterparty") {
        localStorage.setItem("Counterparty", JSON.stringify(this.counterparty));
      } else {
        localStorage.setItem("Organization", JSON.stringify(this.counterparty));
      }
    },

    selectItem() {
      this.getSelectOrganization();
      bus.$emit("select-items", this.counterpartyActiv);
    },
  },
};
</script>