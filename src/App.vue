<template>
  <div id="app" class="container-xl">
    <div>
      <h1 class="text-center">Выставление счетов</h1>
    </div>
    <div>
      <div class="Organization-item">
        <h3 class="">Организация</h3>
        <OrganizationSelected :counterparty="organization" />
      </div>

      <div class="Organization-item">
        <h3 class="">Контрагент</h3>
        <OrganizationSelected :counterparty="counterparty" />
      </div>
    </div>

    <div class="row g-3 Organization-item">
          <div class="col-sm-3">
      <button @click="isVisible = true" class="btn btn-secondary">
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
          v-model="numberDogovor"
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
        <label for="validationDefault04" class="form-label">Ставка НДС</label>
        <select
          v-model="selectNdc"
          class="form-select"
          id="validationDefault04"
        >
          <option>20 %</option>
          <option>10 %</option>
          <option>Без ндс</option>
        </select>
      </div>
    </div>

    <div class="Organization-item">
      <h3>Товары</h3>
      <Tableform :selectNdc="selectNdc" @itemsarray="itemsarray"/>
      
      <PDF :numberDogovor="numberDogovor" :items="items" :dateToday="dateToday" :numberScore="numberScore"/>

      <FormInput :isVisible="isVisible" />
    </div>
  </div>
</template>

<script>
import OrganizationSelected from "@/components/OrganizationSelected.vue";
import FormInput from "@/components/FormInput.vue";
import Tableform from "@/components/Tableform.vue";
import PDF from "@/components/PDF.vue";

export default {
  data() {
    return {
      isVisible: false,
      counterparty: JSON.parse(localStorage.getItem("Counterparty")),
      organization: JSON.parse(localStorage.getItem("Organization")),
      selectNdc: "20 %",
      dateToday: '',
      numberDogovor: '',
      numberScore: '',
      items:[]
    };
  },
  methods: {
    itemsarray(data) {
      this.items = data
    }
  },
  components: {
    OrganizationSelected,
    FormInput,
    Tableform,
    PDF,
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

.Organization-item {
  margin: 5px -20px;
  padding: 20px 20px 20px;
  background: rgba(241, 239, 238, 0.5);
  border-radius: 10px;
}


</style>
