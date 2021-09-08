  <template>
  <form class="g-3" @submit="onSubmit">
    <div v-show="selectVisible">
      <label class="form-label">Вид организации</label>
      <select
        v-model="activList"
        class="form-select"
        :class="{ 'is-invalid': $v.activList.$error }"
        @blur="$v.activList.$touch()"
      >
        <option disabled value="Выберите тип">Выберите тип</option>
        <option value="Organization">Организация</option>
        <option value="Counterparty">Контрагент</option>
      </select>
    </div>

    <div class="row mb-1">
      <label for="fields.name" class="col-sm-5 col-form-label"
        >Название Организации</label
      >
      <div class="col-sm-6">
        <input
          v-model="fields.name"
          id="fields.name"
          class="form-control"
          :class="{ 'is-invalid': $v.fields.name.$error }"
          @blur="$v.fields.name.$touch()"
          :disabled="disabledInput"
        />
      </div>
    </div>
    <div class="row mb-1">
      <label for="fields.inn" class="col-sm-5 col-form-label">ИНН</label>
      <div class="col-sm-6">
        <input
          v-model="fields.inn"
          id="fields.inn"
          class="form-control"
          :class="{ 'is-invalid': $v.fields.inn.$error }"
          @blur="$v.fields.inn.$touch()"
          :disabled="disabledInput"
        />
      </div>
    </div>
    <div class="row mb-1">
      <label for="fields.kpp" class="col-sm-5 col-form-label">КПП</label>
      <div class="col-sm-6">
        <input
          v-model="fields.kpp"
          id="fields.kpp"
          class="form-control"
          :class="{ 'is-invalid': $v.fields.kpp.$error }"
          @blur="$v.fields.kpp.$touch()"
          :disabled="disabledInput"
        />
      </div>
    </div>
    <div class="row mb-1">
      <label for="fields.bik" class="col-sm-5 col-form-label">БИК</label>
      <div class="col-sm-6">
        <input
          v-model="fields.bik"
          id="fields.bik"
          class="form-control"
          :class="{ 'is-invalid': $v.fields.bik.$error }"
          @blur="$v.fields.bik.$touch()"
          :disabled="disabledInput"
        />
      </div>
    </div>
    <div class="row mb-1">
      <label for="fields.nameBank" class="col-sm-5 col-form-label"
        >Название банка</label
      >
      <div class="col-sm-6">
        <input
          v-model="fields.nameBank"
          id="fields.nameBank"
          class="form-control"
          :class="{ 'is-invalid': $v.fields.nameBank.$error }"
          @blur="$v.fields.nameBank.$touch()"
          :disabled="disabledInput"
        />
      </div>
    </div>
    <div class="row mb-1">
      <label for="fields.rasChet" class="col-sm-5 col-form-label"
        >Расч. счет</label
      >
      <div class="col-sm-6">
        <input
          v-model="fields.rasChet"
          id="fields.rasChet"
          class="form-control"
          :class="{ 'is-invalid': $v.fields.rasChet.$error }"
          @blur="$v.fields.rasChet.$touch()"
          :disabled="disabledInput"
        />
      </div>
    </div>
    <div class="row mb-1">
      <label for="fields.korChet" class="col-sm-5 col-form-label"
        >Корр. счет</label
      >
      <div class="col-sm-6">
        <input
          v-model="fields.korChet"
          id="fields.korChet"
          class="form-control"
          :class="{ 'is-invalid': $v.fields.korChet.$error }"
          @blur="$v.fields.korChet.$touch()"
          :disabled="disabledInput"
        />
      </div>
    </div>
    <div class="row mb-1">
      <label for="fields.adress" class="col-sm-5 col-form-label">Адрес</label>
      <div class="col-sm-6">
        <input
          v-model="fields.adress"
          id="fields.adress"
          class="form-control"
          :class="{ 'is-invalid': $v.fields.adress.$error }"
          @blur="$v.fields.adress.$touch()"
          :disabled="disabledInput"
        />
      </div>
    </div>
    <div class="row mb-1">
      <label for="fields.telefon" class="col-sm-5 col-form-label"
        >Телефон</label
      >
      <div class="col-sm-6">
        <input
          v-model="fields.telefon"
          id="fields.telefon"
          class="form-control"
          :class="{ 'is-invalid': $v.fields.telefon.$error }"
          @blur="$v.fields.telefon.$touch()"
          :disabled="disabledInput"
        />
      </div>
    </div>

    <button
      type="submit"
      class="btn btn-secondary footer"
      :disabled="$v.$invalid"
      @click="$emit('close')"
      v-show="buttonVisible"
    >
      Сохранить
    </button>
  </form>
</template>

<script>
import { required } from "vuelidate/lib/validators";

export default {
  props: {
    selectVisible: {
      type: Boolean,
      default: true,
    },
    buttonVisible: {
      type: Boolean,
      default: true,
    },
    fillingCounterparty: {
      type: Object,
      default: () => {},
    },
    disabledInput: {
      type: Boolean,
      default: false  
    }
  },
  data() {
    return {
      fields: {
        name: "",
        inn: "",
        kpp: "",
        bik: "",
        nameBank: "",
        rasChet: "",
        korChet: "",
        adress: "",
        telefon: "",
      },
      elProps: {},
      listOrgCount: "",
      activList: "Выберите тип",
    };
  },
  validations: {
    activList: {
      selectNotSelected: (data) => {
        return data !== "Выберите тип";
      },
    },
    fields: {
      name: { required },
      inn: { required },
      kpp: { required },
      bik: { required },
      nameBank: { required },
      rasChet: { required },
      korChet: { required },
      adress: { required },
      telefon: { required },
    },
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
      if (this.fillingCounterparty != undefined) {
        let list = JSON.parse(
            localStorage.getItem(`${this.fillingCounterparty.listOrgCount}`)
        );
        list.forEach((element, index) => {
          if (element.fields.name == this.fillingCounterparty.fields.name) {
            list.splice(index, 1);
          }
          localStorage.setItem(
              this.fillingCounterparty.listOrgCount,
              JSON.stringify(list)
          );
        });
      }
    },
    onSubmit() {
      this.deleteOldcounterparty();
      let listOrg = {
        listOrgCount: (this.listOrgCount = this.activList),
        fields: {
          name: this.fields.name,
          inn: this.fields.inn,
          kpp: this.fields.kpp,
          bik: this.fields.bik,
          nameBank: this.fields.nameBank,
          rasChet: this.fields.rasChet,
          korChet: this.fields.korChet,
          adress: this.fields.adress,
          telefon: this.fields.telefon,
        },
      };
      if (this.listOrgCount === "Counterparty") {
        this.pushOrganization(this.listOrgCount, listOrg);
      } else {
        this.pushOrganization(this.listOrgCount, listOrg);
      }
    },
  },
  mounted() {
        if (Object.keys(this.fillingCounterparty).length !== 0) {
          this.fields = {...this.fillingCounterparty.fields}
          }
    },
};
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
}
select {
  margin-bottom: 1rem;
}
</style>