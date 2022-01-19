<template>
  <div class="condition p-5 bg-opacity-10" :class="Classes()">
    <div class="condition__type d-flex justify-content-between mb-2">
      <h2 class="condition__title h6">Условие {{ number }}</h2>
      <select v-model="conditionType" class="form-select w-75 col-md-8">
        <option
          v-for="condition in Object.keys(conditions)"
          :key="condition"
          :value="condition"
          :selected="condition == 'default' ? true : false"
        >
          {{ conditions[condition] }}
        </option>
      </select>
    </div>
    <div v-for="key in count.length" :key="key" class="condition__params">
      <TypeSelect
        v-if="conditionType"
        :params="getProps()"
        :number="key"
        :operator="opratorReturn(key)"
      />
    </div>
    <div
      class="condition__button-wrapper d-flex justify-content-between col-md-8 ml-auto p-0"
    >
      <button
        type="button"
        class="btn btn-outline-success"
        @click="coununtIncrees()"
      >
        Добавить {{ getProps() ? getProps().label : '' }}
      </button>

      <div class="condition__operators" :class="{ 'd-none': !operatorsShow }">
        <button
          class="condition__operator mr-2 btn btn-outline-success"
          type="button"
          @click="coununtIncreesWithOperator('Или')"
        >
          Или
        </button>
        <button
          class="condition__operator btn btn-outline-success"
          type="button"
          @click="coununtIncreesWithOperator('И')"
        >
          И
        </button>
      </div>

      <button
        type="button"
        class="btn btn-outline-danger"
        @click="coununtDecrees()"
      >
        Удалить {{ getProps() ? getProps().label : '' }}
      </button>
    </div>
    <div
      v-if="number !== 1"
      class="condition__main-operator bg-success bg-opacity-25"
    >
      И
    </div>
  </div>
</template>

<script>
import TypeSelect from './ConditionParamTypes/TypeSelect.vue'
export default {
  name: 'ConditionItem',

  components: { TypeSelect },

  props: {
    conditions: {
      required: true,
      type: Object,
    },
    number: {
      type: Number,
      default: 1,
    },
  },

  data() {
    return {
      conditionType: null,
      conditionProps: {
        label: 'Тип',
      },
      count: [],
      operatorsShow: false,
    }
  },
  watch: {
    conditionType() {
      this.count.length = 0
    },
  },
  methods: {
    opratorReturn(key) {
      if (!key || key === 1) return null
      return this.count[key - 1]
    },
    coununtIncrees() {
      if (!this.conditionType) return
      if (!this.count.length) this.count.push(null)
      else {
        this.operatorsShow = true
      }
    },
    coununtIncreesWithOperator(operator) {
      this.count.push(operator)
      this.operatorsShow = false
    },
    coununtDecrees() {
      if (this.conditionType) {
        this.count.pop()
      }
    },
    Classes() {
      return {
        'border-top': this.number !== 1,
        'background-success': this.number % 3 < 1,
        'background-warning': (this.number + 1) % 3 < 1,
        'background-danger': (this.number + 2) % 3 < 1,
      }
    },
    getProps() {
      const type = this.conditionType

      if (!type) {
        return null
      }

      switch (type) {
        case 'type':
          return { label: 'Тип', fileName: 'MySelect', count: this.count }
        case 'status':
          return { label: 'Сатаус', fileName: 'MySelect', count: this.count }
        case 'age':
          return { label: 'Диапазон', fileName: 'MyRange', count: this.count }
      }
    },
  },
}
</script>

<style lang="scss">
.condition {
  position: relative;

  & .background-success {
    background: rgba(93, 255, 206, 0.26);
  }
  & .background-warning {
    background: rgba(255, 244, 93, 0.26);
  }
  & .background-danger {
    background: rgba(255, 125, 93, 0.26);
  }
  &__main-operator {
    position: absolute;
    top: 0;
    transform: translate(0, -50%);
    padding: 10px;
    border-radius: 3px;
  }
}
</style>
