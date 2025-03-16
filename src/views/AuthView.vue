<template>
  <div class="registration-container">
    <h1>Форма регистрации</h1>

    <app-form
      ref="childRef"
      :is-submitting="isSubmitting"
      @submit="formSubmit"
    />

    <div v-if="responseError" class="error-message">
      {{ responseError }}
    </div>

    <div v-if="responseData" class="response-message">
      {{ responseData }}
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import AppForm from '@/components/AppForm.vue';
import { FormFields } from '@/shared/form-interface';

export default defineComponent({
  name: 'app-auth-page',
  components: { AppForm },

  setup() {
    const isSubmitting = ref<boolean>(false);

    const responseError = ref<string>('');
    const responseData = ref<string>('');

    const childRef = ref<InstanceType<typeof AppForm> | null>(null);

    return {
      isSubmitting,
      childRef,
      responseError,
      responseData,
    };
  },

  methods: {
    formSubmit(formData: FormFields) {
      this.isSubmitting = true;

      const credentials = { username: formData.firstName, password: formData.password };
      fetch('https://fakestoreapi.com/auth/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(credentials),
      }).then((response) => response.json())
        .then((data) => {
          this.responseError = '';
          this.responseData = JSON.stringify({
            ...data,
            username: formData.firstName,
          });
        })
        .catch(() => {
          this.responseError = 'Для успешного входа Имя: "johnd", Пароль: "m38rmF$"';
        })
        .finally(() => {
          this.isSubmitting = false;

          if (this.childRef) {
            this.childRef.resetForm();
          }
        });
    },
  },

});
</script>

<style>
.registration-container {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
  color: #95b8e2;
  margin-bottom: 30px;
}

.error-message {
  margin-top: 20px;
  padding: 15px;
  background-color: #e74c3c;
  color: white;
  border-radius: 4px;
  text-align: center;
}

.response-message {
  margin-top: 20px;
  padding: 15px;
  background-color: #58d98f;
  color: #272b34;
  border-radius: 4px;
  text-align: center;
  width: 100%;
  word-break: break-all;
}
</style>
