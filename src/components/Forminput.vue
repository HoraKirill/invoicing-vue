  <template>
  <form v-show="isVisible" class="showModal">
    <div class="header">
      <h3>Данные Организации</h3>
      <button @close="isVisible = !isVisible" class="btn-close">x</button>
    </div>
    <div v-show="selectVisible">
      <select v-model="activList" >
        <option disabled value="">Выберите тип</option>
        <option value="Organization">Организация</option>
        <option value="Counterparty">Контрагент</option>
      </select>
       Вид организации
    </div>

    <label><input v-model="name" /> Название Организации </label>
    <label> <input v-model="inn" /> ИНН </label>
    <label> <input v-model="kpp" /> КПП</label>
    <label> <input v-model="bik" /> БИК </label>
    <label> <input v-model="nameBank" /> Название банка </label>
    <label> <input v-model="rasChet" /> Расч. счет </label>
    <label> <input v-model="korChet" /> Корр. счет </label>
    <label> <input v-model="adress" /> Адресс </label>
    <label> <input v-model="telefon" /> Телефон </label>
    <br />

    <button class="btn btn-secondary"
      @click="submit"
      @close="isVisible = !isVisible"
      v-show="buttonVisible"
    >
      Сохранить
    </button>
  </form>
</template>

<script>
export default {
  props: {
    selectVisible: {
      default: true,
    },
    buttonVisible: {
      default: true,
    },
    isVisible: {
      default: false,
    },
    counterparty: {
      type: Object,
      default: () => {}
    },
  },
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
      activList: "Выберите тип"
    };
  },
  methods: {
    pushOrganization(item, arrList) {
      if (localStorage.getItem(`${item}`) !== null) {
        let list = JSON.parse(localStorage.getItem(`${item}`));
        list.push(arrList);
        localStorage.setItem(item, JSON.stringify(list));
      } else {
        let list = [arrList];
        localStorage.setItem(item, JSON.stringify(list));
      }
    },
    deleteOldcounterparty() {
      if (this.counterparty != undefined) {
        let list = JSON.parse(
          localStorage.getItem(`${this.counterparty.listOrgCount}`)
        );
        list.forEach((element, index) => {
          if (element.name == this.counterparty.name) {
            list.splice(index, 1);
          }
          localStorage.setItem(
            this.counterparty.listOrgCount,
            JSON.stringify(list)
          );
        });
      }
    },
    submit() {
      if (this.activList === "Выберите тип") {
        alert("Выберете тип организации")
        event.preventDefault();
      }
      this.deleteOldcounterparty();
      let listOrg = {
        listOrgCount: this.listOrgCount = this.activList,
        name: this.name,
        inn: this.inn,
        kpp: this.kpp,
        bik: this.bik,
        nameBank: this.nameBank,
        rasChet: this.rasChet,
        korChet: this.korChet,
        adress: this.adress,
        telefon: this.telefon,
      };
      if (this.listOrgCount === "Counterparty") {
        this.pushOrganization(this.listOrgCount, listOrg);
      } else {
        this.pushOrganization(this.listOrgCount, listOrg);
      }
    },
  },
  watch: {
    counterparty: function () {
      if (this.counterparty != undefined) {
        (this.name = this.counterparty.name),
          (this.inn = this.counterparty.inn),
          (this.kpp = this.counterparty.kpp),
          (this.bik = this.counterparty.bik),
          (this.nameBank = this.counterparty.nameBank),
          (this.rasChet = this.counterparty.rasChet),
          (this.korChet = this.counterparty.korChet),
          (this.rasChet = this.counterparty.rasChet),
          (this.korChet = this.counterparty.korChet),
          (this.adress = this.counterparty.adress),
          (this.telefon = this.counterparty.telefon);
      }
    },
  },
};
</script>

<style scoped>
.showModal {
  display: block;
  position: fixed;
  z-index: 100;
  left: 35%;
  top: 50px;
  width: 500px;
  margin: 5px 5px;
  padding: 20px 30px;
  background-color: #f8f9fa;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  font-family: Helvetica, Arial, sans-serif;
  text-align: left;
  border-radius: 10px;
}

.btn-close {
  background: #ff0000;
  height: 30px;
  width: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
  border: none;
  position: absolute;
  right: 0;
  top: 0;
  background: none;
  cursor: pointer;
  transition: 0.3s;
  outline: none;
}

input {
  margin: 2px 3px 3px 0px;
}
</style>