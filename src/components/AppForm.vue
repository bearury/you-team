<template>
  <form @submit.prevent="submitForm">
    <div class="form-group">
      <app-input
        id="firstName"
        v-model="form.firstName"
        :error="errors.firstName"
        label="Имя*"
        type="text"
        @blur="blur"
      />
    </div>

    <div class="form-group">
      <app-input
        id="lastName"
        v-model="form.lastName"
        :error="errors.lastName"
        label="Фимилия*"
        type="text"
        @blur="blur"
      />
    </div>

    <div class="form-group">
      <app-input
        id="phone"
        v-model="form.phone"
        :error="errors.phone"
        label="Номер телефона*"
        type="tel"
        @blur="blur"
      />
    </div>

    <div class="form-group">
      <app-input
        id="email"
        v-model="form.email"
        :error="errors.email"
        label="Почта*"
        type="email"
        @blur="blur"
      />
    </div>

    <div class="form-group">
      <app-input
        id="password"
        v-model="form.password"
        :error="errors.password"
        label="Пароль*"
        type="password"
        @blur="blur"
      />
    </div>

    <div class="form-group">
      <app-input
        id="repeatPassword"
        v-model="form.repeatPassword"
        :error="errors.repeatPassword"
        label="Повторите пароль*"
        type="password"
        @blur="blur"
      />
    </div>

    <app-button
      :disabled="isSubmitting"
      type="submit">
      {{ isSubmitting ? 'Отправка...' : 'Регистрация' }}
    </app-button>
  </form>
</template>

<script lang="ts">

import { defineComponent, reactive } from 'vue';
import { FormFields } from '@/shared/form-interface';
import AppInput from '@/components/AppInput.vue';
import AppButton from '@/components/AppButton.vue';

export default defineComponent({
  name: 'app-form',
  components: { AppButton, AppInput },

  props: {
    isSubmitting: {
      type: Boolean,
      required: true,
    },
  },

  methods: {
    blur(key: keyof FormFields) {
      this.validateField(key);
    },
    submitForm() {
      if (this.validateForm()) {
        this.$emit('submit', this.form);
      }
    },
    resetForm() {
      this.form.firstName = '';
      this.form.lastName = '';
      this.form.email = '';
      this.form.password = '';
      this.form.phone = '';
      this.form.repeatPassword = '';
    },
  },
  emits: ['submit'],
  setup() {
    const form = reactive<FormFields>({
      firstName: '',
      lastName: '',
      phone: '',
      email: '',
      password: '',
      repeatPassword: '',
    });

    const errors = reactive<FormFields>({
      firstName: '',
      lastName: '',
      phone: '',
      email: '',
      password: '',
      repeatPassword: '',
    });

    const validateField = (field: keyof FormFields) => {
      errors[field] = '';

      switch (field) {
        case 'firstName':
          if (!form.firstName) {
            errors.firstName = 'Поле обязательно для заполнения';
          } else if (form.firstName.length < 2) {
            errors.firstName = 'Должно быть не менее двух символов';
          }
          break;
        case 'lastName':
          if (!form.lastName) {
            errors.lastName = 'Поле обязательно для заполнения';
          } else if (form.lastName.length < 2) {
            errors.lastName = 'Должно быть не менее двух символов';
          }
          break;

        case 'phone':
          if (!form.phone) {
            errors.phone = 'Поле обязательно для заполнения';
          } else if (!/(^(?:\+7|8)\d{10}$)/.test(form.phone)) {
            errors.phone = 'Ведите номер телефона в формате +7(8)xxx xxx xxxx';
          }
          break;

        case 'email':
          if (!form.email) {
            errors.email = 'Поле обязательно для заполнения';
          } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(form.email)) {
            errors.email = 'Введите адрес в правильном формате';
          }
          break;

        case 'password':
          if (!form.password) {
            errors.password = 'Поле обязательно для заполнения';
          } else if (form.password.length < 7) {
            errors.password = 'Пароль должен быть не менее 8 символов';
          } else if (!/[A-ZА-Я]/.test(form.password)) {
            errors.password = 'Пароль должен сожержать не менее оной заглавной буквы';
          } else if (!/[0-9]/.test(form.password)) {
            errors.password = 'Пароль должен содержать не менее одной цифры';
          }
          break;

        case 'repeatPassword':
          if (!form.repeatPassword) {
            errors.repeatPassword = 'Подтвердите пароль';
          } else if (form.password !== form.repeatPassword) {
            errors.repeatPassword = 'Пароли не совпадают';
          }
          break;
        default:
          break;
      }
    };

    const validateForm = (): boolean => {
      validateField('firstName');
      validateField('lastName');
      validateField('phone');
      validateField('email');
      validateField('password');
      validateField('repeatPassword');
      return !Object.values(errors).some((error) => error !== '');
    };

    return {
      form,
      errors,
      validateField,
      validateForm,
    };
  },
});
</script>

<style scoped>
.form-group {
  margin-bottom: 20px;
}
</style>
