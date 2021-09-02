<template>
  <div class="form-label-margin-bottom " >
    <select v-model="select" @click="selectItem">
      <option disabled value="">Выберите ораганизацию</option>
      <option v-for="listOoo in counterparty" :key="listOoo.name">
        {{ listOoo.name }}
      </option>
    </select>
    <button @click="watch()" class="btn btn-sm">Посмотреть</button>
    <button @click="change()" class="btn btn-sm">Изменить</button>
    <button @click="deleteOrg()" class="btn btn-sm">Удалить</button>

    <Forminput
      :isVisible="isVisible"
      :counterparty="counterpartyActiv"
      :selectVisible="selectVisible"
      :buttonVisible="buttonVisible"
    />
  </div>
</template>

<script>
import Forminput from "@/components/Forminput.vue";
import { bus } from '../main';

export default {
  name: "OrganizationSelected",
  props: ["counterparty"],
  data() {
    return {
      isVisible: false,
      select: "",
      counterpartyActiv: [],
      selectVisible: true,
      buttonVisible: true,
    };
  },
  components: {
    Forminput,
  },
  methods: {
    getSelectOrganization () {
      if (this.counterparty !== null )
      this.counterparty.forEach((element) => {
        if (element.name === this.select) {
         return this.counterpartyActiv = element;
        }
      });
    },

    watch() {
      this.isVisible = true;
      this.getSelectOrganization ()
      this.selectVisible = false;
      this.buttonVisible = false;
     
    },

    change() {
      this.isVisible = true;
      this.getSelectOrganization ()
    },

    deleteOrg() {
      let listOrgCount = this.counterparty.listOrgCount
      this.counterparty.forEach((element, index) => {
        if (element.name === this.select) {
          this.counterparty.splice(index, 1)
          }
      });
      if (listOrgCount === "Counterparty") {
        localStorage.setItem("Counterparty", JSON.stringify(this.counterparty));
      } else {
        localStorage.setItem("Organization", JSON.stringify(this.counterparty));
      }
    },

    selectItem() {
      this.getSelectOrganization ()
      bus.$emit('select-items', this.counterpartyActiv)
    }
  },
};
</script>