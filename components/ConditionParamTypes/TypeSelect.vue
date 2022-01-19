<template>
  <div class="condition__param d-flex mb-2">
    <label class="contition-param__label col-md-4">
      <span v-if="operator" class="mr-2">{{ operator }}</span
      >{{ params.label }} {{ number }}
    </label>

    <component :is="componentInstance"></component>
  </div>
</template>

<script>
export default {
  name: 'CardType',

  props: {
    params: {
      required: true,
      type: Object,
    },
    number: {
      type: Number,
      default: 1,
    },
    operator: {
      type: String,
      required: true,
    },
  },

  computed: {
    componentInstance() {
      if (this.params.fileName) {
        return () => import(`./${this.params.fileName}`)
      }
      return null
    },
  },
}
</script>
