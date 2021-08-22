<template>
  <div class="OrganizationSelected">
    <select>
      <option v-for="listOoo in counterparty" :key="listOoo.name">
        {{ listOoo.name }}
      </option>
    </select>
    <button>Посмотреть</button>
    <button>Изменить</button>
    <button>Удалить</button>
  </div>
</template>

<script>
export default {
  name: "OrganizationSelected",
  props: ["counterparty"],
  data() {
    return {};
  },
};
</script>