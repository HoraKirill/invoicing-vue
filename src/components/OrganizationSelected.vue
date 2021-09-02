<template>
  <div class="form-label-margin-bottom">
    <select v-model="select" @click="selectItem">
      <option disabled value="">Выберите ораганизацию</option>
      <option v-for="listOoo in counterparty" :key="listOoo.fileds.name">
        {{ listOoo.fileds.name }}
      </option>
    </select>
    <button @click="watchModal()" class="btn btn-sm">Посмотреть</button>
    <button @click="change()" class="btn btn-sm">Изменить</button>
    <button @click="deleteOrg()" class="btn btn-sm">Удалить</button>

    <FormInput
      :isVisible="isVisible"
      :counterparty="counterpartyActiv"
      :selectVisible="selectVisible"
      :buttonVisible="buttonVisible"
    />
  </div>
</template>

<script>
import FormInput from "@/components/FormInput.vue";
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
      isVisible: false,
      select: "",
      counterpartyActiv: {},
      selectVisible: true,
      buttonVisible: true,
    };
  },
  components: {
    FormInput,
  },
  methods: {
    getSelectOrganization() {
      if (this.counterparty !== null)
        this.counterparty.forEach((element) => {
          if (element.fileds.name === this.select) {
            return (this.counterpartyActiv = element);
          }
        });
    },

    watchModal() {
      this.isVisible = true;
      this.getSelectOrganization();
      this.selectVisible = false;
      this.buttonVisible = false;
    },

    change() {
      this.isVisible = true;
      this.getSelectOrganization();
    },

    deleteOrg() {
      let listOrgCount = this.counterparty.listOrgCount;
      const idx = this.counterparty.findIndex((i) => i.fileds.name === this.select);
      if (idx >= 0) {
        this.counterparty.fileds.splice(idx, 1);
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