<template>
  <form v-show="isVisible" class="showModal">
    <h3>Данные Организации</h3>
    <select v-model="listOrgCount">
      <option value="Organization">Организация</option>
      <option value="Counterparty">Контрагент</option>
    </select>

    <label> <input v-model="name" /> Название Организации </label>
    <label> <input v-model="inn" /> ИНН </label>
    <label> <input v-model="kpp" /> КПП</label>
    <label> <input v-model="bik" /> БИК </label>
    <label> <input v-model="nameBank" /> Название банка </label>
    <label> <input v-model="rasChet" /> Расч. счет </label>
    <label> <input v-model="korChet" /> Корр. счет </label>
    <label> <input v-model="adress" /> Адресс </label>
    <label> <input v-model="telefon" /> Телефон </label>
    <br />
    <button @click="submit" @close="isVisible = !isVisible">Сохранить</button>
  </form>
</template>

<script>
export default {
  props: ["isVisible"],
  data() {
    return {
      name: "",
      inn: "",
      kpp: "",
      bik: "",
      nameBank: "",
      rasChet: "",
      korChet: "",
      adress: "",
      telefon: "",
      listOrgCount: "",
    };
  },
  methods: {
    submit: function () {
      let listOrg = {
        listOrgCount: this.listOrgCount,
        name: this.name,
        inn: this.inn,
        kpp: this.kpp,
        bik: this.bik,
        nameBank: this.nameBank,
        rasChet: this.rasChet,
        korChet: this.korChet,
        adress: this.adress,
        telefon: this.telefon,
      }
      if (this.listOrgCount === "Counterparty") {
         if (localStorage.getItem('Counterparty') !== null) {
        let list = JSON.parse(localStorage.getItem("Counterparty")) 
        list.push(listOrg)
        localStorage.setItem("Counterparty", JSON.stringify(list))
        } else {
          let list = [listOrg]
        localStorage.setItem("Counterparty", JSON.stringify(list))
        }
      } else {
         if (localStorage.getItem('Organization') !== null) {
        let list = JSON.parse(localStorage.getItem("Organization")) 
        list.push(listOrg)
        localStorage.setItem("Organization", JSON.stringify(list))
        } else {
          let list = [listOrg]
        localStorage.setItem("Organization", JSON.stringify(list))
        }
      }
    },
  },
};
</script>

<style scoped>
.showModal {
  border-radius: 2px;
  width: 500px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: rgb(199, 225, 235);
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
}

label {
  display: block;
}

input {
  width: 50%;
  height: 20px;
}
</style>