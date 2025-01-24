<template>
  <div>
    <div class="select">
      Active Option
      <select :name="field.id" :disabled="field.disabled ?? null" multiple="true" >

        >
        <option v-for="option in optionEnable" @click="updateSelectDisabled(option.id)" :key="option.value"
          :value="option.id">{{ option.label }}</option>
      </select>
    </div>

    <div class="select">
      Disable Option
      <select :name="`${field.id}-2`" :disabled="field.disabled ?? null" multiple="true">
        <option v-for="option in optionDisable" @click="updateSelectActive(option.id)" :key="option.value"
          :value="option.value">{{ option.label }}</option>
      </select>
    </div>
  </div>

</template>

<script setup>
import { ref } from 'vue';
import fieldMixin from '../FieldMixin';
const emit = defineEmits(['update']);
const props = defineProps({
  field: {
    type: Object,
    required: true,
  },
  modelValue: {
    type: [String, Number, Boolean, Object, Array],
    default: '',
  },
});


const optionEnable = ref(props.field.options);
const optionDisable = ref([]);

const updateSelectDisabled = (idOption) => {
  const index = optionEnable.value.findIndex(option => option.id === idOption);
  if (index !== -1) {
    optionDisable.value.push(optionEnable.value[index]);
    optionEnable.value.splice(index, 1);
  }
}

const updateSelectActive = (idOption) => {
  const index = optionDisable.value.findIndex(option => option.id === idOption);
  if (index !== -1) {
    optionEnable.value.push(optionDisable.value[index]);
    optionDisable.value.splice(index, 1);
  }
  updateData(optionEnable)
}




let { handleChange, debounce } = fieldMixin.setup(props, { emit });
const updateData = (event) => {
  debounce(handleChange, 50)(event);
};
</script>

<style scoped>
div {
  display: flex;
  gap: 40px;
  margin-bottom: 10px;
  margin-top: 10px;
}

div div.select {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

div select {
  width: 200px;
  padding: 2px 10px;
  border: 1px solid #ccc;
  height: 120px;
}
</style>