<template>
  <div id="app" class="container-md">
    <div>
      <h1 class="text-center">Выставление счетов</h1>
    </div>
    <div>
      <div class="organizationItem">
        <h3 class="">Организация</h3>
        <OrganizationСhanges :counterparty="organization" />
      </div>

      <div class="organizationItem">
        <h3 class="">Контрагент</h3>
        <OrganizationСhanges :counterparty="counterparty" />
      </div>
    </div>

    <div class="row g-3 organizationItem">
      <div class="col-sm-3">
        <button
          @click="showModal = true"
          type="button"
          class="btn btn-secondary"
         >
          Создать Организацию или Контрагента
        </button>
      </div>
      <div class="col-sm-2">
        <label for="validationDefault01" class="form-label">Дата</label>
        <input
          v-model="dateToday"
          type="date"
          class="form-control"
          id="validationDefault01"
          required
        />
      </div>
      <div class="col-sm-2">
        <label for="validationDefault02" class="form-label"
          >Номер договора</label
        >
        <input
          v-model="contractNumber"
          type="text"
          class="form-control"
          id="validationDefault02"
          required
        />
      </div>
      <div class="col-sm-2">
        <label for="validationDefault03" class="form-label">Номер счета</label>
        <input
          v-model="numberScore"
          type="text"
          class="form-control"
          id="validationDefault03"
          required
        />
      </div>
      <div class="col-sm-2">
        <label class="form-label">Ставка НДС</label>
        <select
          v-model="selectNdc"
          class="form-select"
        >
          <option>20 %</option>
          <option>10 %</option>
          <option>Без ндс</option>
        </select>
      </div>
    </div>

    <div class="organizationItem">
      <h3>Товары</h3>
      <TableForm 
      :selectNdc="selectNdc" 
      @itemsarray="itemsArray" />

      <PDF
        :contractNumber="contractNumber"
        :items="items"
        :dateToday="dateToday"
        :numberScore="numberScore"
      />

      <ModalWindow 
      v-if="showModal" 
      @close="showModal = false">
        <FormInput 
        slot="body"  
        @close="showModal = false"
        />
        <h2 slot="header">Данные Организации</h2>
      </ModalWindow>
    </div>
  </div>
</template>

<script>
import OrganizationСhanges from "@/components/OrganizationСhanges.vue";
import FormInput from "@/components/FormInput.vue";
import TableForm from "@/components/TableForm.vue";
import PDF from "@/components/PDF.vue";
import ModalWindow from "@/components/ModalWindow.vue";

export default {
  data() {
    return {
      counterparty: JSON.parse(localStorage.getItem("Counterparty")),
      organization: JSON.parse(localStorage.getItem("Organization")),
      selectNdc: "20 %",
      dateToday: "",
      contractNumber: "",
      numberScore: "",
      items: [],
      showModal: false
    };
  },
  methods: {
    itemsArray(data) {
      this.items = data;
    },
  },
  components: {
    OrganizationСhanges,
    FormInput,
    TableForm,
    PDF,
    ModalWindow,
  },
};
</script>

<style>
body {
  color: #222;
  font-size: 17px;
}

h1 {
  padding-top: 30px;
}

.organizationItem {
  margin: 5px -20px;
  padding: 20px 20px 20px;
  background: rgba(241, 239, 238, 0.5);
  border-radius: 10px;
}
</style>
