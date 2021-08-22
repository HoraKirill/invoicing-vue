<template>
  <div id="app">
    <div>
      <h1>Выставление счетов</h1>
    </div>
    <div class="Organization">
      <div class="Organization-item">
        <h2>Организация</h2>
        <OrganizationSelected :counterparty="organization" />
      </div>
      <div class="Organization-item">
        <h2>Контрагент</h2>
        <OrganizationSelected :counterparty="counterparty" />
      </div>
    </div>
    <div>
      <button @click="isVisible = true">
        Создать Организацию или Контрагента
      </button>
    </div>
    <h2>Товары</h2>
    <div class="inputDate">
      <input type="date">
      <input type="text" placeholder="Номер договора">
      <input type="text" placeholder="Номер счета">
    </div>
    <Tableform />
    <Forminput :isVisible="isVisible" />
  </div>
</template>

<script>
import OrganizationSelected from "@/components/OrganizationSelected.vue";
import Forminput from "@/components/Forminput.vue";
import Tableform from "@/components/Tableform.vue";

export default {
  data() {
    return {
      isVisible: false,
      counterparty: JSON.parse(localStorage.getItem('Counterparty')),
      organization: JSON.parse(localStorage.getItem('Organization'))
    }
  },

  components: {
    OrganizationSelected,
    Forminput,
    Tableform
  },
};
</script>

<style>
.inputDate input{
  margin: 5px 5px 5px 5px;
}

h1 {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 30px;
}

h2 {
  text-align: center;
}

.Organization {
  display: inline-flex;
  width: 50%;
}

.Organization-item {
  padding: 5px;
  margin-bottom: 10px;
}
</style>
