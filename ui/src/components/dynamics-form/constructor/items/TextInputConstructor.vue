<template>
  <el-form-item label="取值范围" required>
    <el-col :span="11" style="padding-left: 0">
      <el-input-number
        style="width: 100%"
        v-model="formValue.min_length"
        controls-position="right"
      />
    </el-col>
    <el-col :span="2" class="text-center">
      <span class="text-gray-500">-</span>
    </el-col>
    <el-col :span="11">
      <el-input-number
        style="width: 100%"
        v-model="formValue.max_length"
        controls-position="right"
      />
    </el-col>
  </el-form-item>

  <el-form-item
    label="默认值"
    :required="formValue.required"
    prop="default_value"
    :rules="formValue.required ? [{ required: true, message: '默认值 为必填属性' }] : []"
  >
    <el-input
      v-model="formValue.default_value"
      :maxlength="formValue.max_length"
      :minlength="formValue.min_length"
      placeholder="请输入默认值"
      show-word-limit
      type="text"
    />
  </el-form-item>
</template>
<script setup lang="ts">
import { computed, onMounted } from 'vue'

const props = defineProps<{
  modelValue: any
}>()
const emit = defineEmits(['update:modelValue'])
const formValue = computed({
  set: (item) => {
    emit('update:modelValue', item)
  },
  get: () => {
    return props.modelValue
  }
})

const getData = () => {
  return {
    input_type: 'TextInput',
    attrs: {
      maxlength: formValue.value.max_length,
      minlength: formValue.value.min_length,
      'show-word-limit': true
    },
    default_value: formValue.value.default_value
  }
}
const rander = (form_data: any) => {
  const attrs = form_data.attrs || {}
  formValue.value.min_length = attrs.min_length
  formValue.value.max_length = attrs.max_length
  formValue.value.default_value = form_data.default_value
}
defineExpose({ getData, rander })
onMounted(() => {
  formValue.value.min_length = 0
  formValue.value.max_length = 20
  formValue.value.default_value = ''
})
</script>
<style lang="scss"></style>
