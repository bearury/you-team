<template>
  <label :for="id">{{ label }}
    <input
      :id="id"
      :placeholder="placeholder"
      :type="type"
      :value="modelValue"
      @blur="$emit('blur', id)"
      @input="change"
    />
  </label>
  <span v-if="error" class="error">{{ error }}</span>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { FormFields } from '@/shared/form-interface';

export default defineComponent({
  name: 'app-input',

  props: {
    modelValue: [String, Number],
    id: {
      type: String as () => keyof FormFields,
      required: true,
    },
    label: {
      type: String,
    },
    type: {
      type: String,
      required: true,
    },
    error: {
      type: String,
    },
    placeholder: String,
  },

  emits: ['update:modelValue', 'blur'],

  methods: {
    change(event:Event) {
      const target = event.target as HTMLInputElement;
      this.$emit('update:modelValue', target.value);
    },
  },
});
</script>

<style scoped>
label {
  display: block;
  font-weight: bold;
  color: #95b8e2;
}

input {
  width: 100%;
  margin-top: .3rem;
  border: 1px solid slateblue;
  padding: .5rem 1rem;
  border-radius: .2rem;
  color: #95b8e2;
  background-color: rgba(255,255,255, 0.1);
  font-size: 1rem;
}

input:-webkit-autofill,
input:-webkit-autofill:hover,
input:-webkit-autofill:focus,
textarea:-webkit-autofill,
textarea:-webkit-autofill:hover,
textarea:-webkit-autofill:focus,
select:-webkit-autofill,
select:-webkit-autofill:hover,
select:-webkit-autofill:focus {
  border: 1px solid slateblue;
  -webkit-text-fill-color: #95b8e2;
  transition: background-color 5000s ease-in-out 0s;
}

input:focus {
  outline: 2px solid slateblue;
  box-shadow: 0 0 5px rgba(74, 144, 226, 0.3);
}

.error {
  color: #e74c3c;
  font-size: 14px;
  margin-top: 5px;
  display: block;
}
</style>
