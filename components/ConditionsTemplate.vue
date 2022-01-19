<template>
  <div class="condition mb-5">
    <h1 class="condition__name h5 mb-5">Добавить опрос</h1>
    <ConditionItem
      v-for="count in amount"
      :key="count"
      :conditions="conditions"
      :number="count"
    />
    <div class="condition__add-btn col-sm-12 mb-4" @click="amount++">
      Добавить условие <br />
      все условия связываются логическим "И"
    </div>
    <div class="condition__button-wrapper d-flex justify-content-between p-0">
      <button type="button" class="btn btn-outline-success">
        Протестировать
      </button>
      <button type="button" class="btn btn-outline-danger">Далее</button>
    </div>
  </div>
</template>

<script>
import ConditionItem from '~/components/ConditionItem.vue'
export default {
  name: 'ContionTemplate',
  components: { ConditionItem },
  data() {
    return {
      conditions: {
        // default: 'Выберите условие',
        type: 'Тип карты лояльности',
        status: 'Статус карты лояльности',
        age: 'Возраст респондента',
      },
      amount: 1,
    }
  },
}
</script>

<style scoped lang="scss">
.condition {
  &__add-btn {
    height: 200px;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    border: 1px solid green;
    text-align: center;
    transition: all ease 0.5s;

    &:hover {
      color: green;
      background: #b1cab1;
    }
  }
}
</style>
