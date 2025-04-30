<template>
  <q-layout>
    <q-page-container>
      <q-page class="flex justify-center items-center bg-primary" padding>
        <q-card class="bg-white">
          <q-card-section>
            <div class="text-h6">Login</div>
          </q-card-section>
          <q-separator />
          <q-card-section>
            <q-form greedy class="q-gutter-y-md" ref="loginForm">
              <q-input
                v-model="username"
                label="Username"
                outlined
                :rules="[(val: string) => !!val || 'Usuário é obrigatório']"
              />
              <password-input
                v-model="password"
                label="Senha"
                outlined
                :rules="[
                  (val: string) => !!val || 'Por favor, insira sua senha',
                  (val: string) => val.length >= 8 || 'Senha deve ter pelo menos 8 caracteres',
                ]"
              />
            </q-form>
          </q-card-section>
          <q-separator />
          <q-card-actions align="evenly" class="row">
            <q-btn class="col-5" label="Esqueceu a senha?" size="sm" flat />
            <q-btn class="col-5" label="Login" color="primary" @click="onLogin" />
          </q-card-actions>
        </q-card>
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import type { QForm } from 'quasar';
import { useAuthStore } from 'src/stores/auth';
import { useQuasar } from 'quasar';

export default defineComponent({
  name: 'LoginPage',
  setup() {
    const authStore = useAuthStore();
    const { notify } = useQuasar();

    const loginForm = ref<QForm>();

    const username = ref<string>('');
    const password = ref<string>('');

    const onLogin = async () => {
      if (loginForm.value && !(await loginForm.value?.validate(true))) {
        return;
      }
      const user = await authStore.login(username.value, password.value);
      if (!user) {
        notify({
          message: 'Usuário ou senha inválidos',
          type: 'negative',
        });
        return;
      }
      notify({
        message: `Bem-vindo ${user.username}`,
        type: 'positive',
      });
    };

    return {
      loginForm,
      username,
      password,
      onLogin,
    };
  },
});
</script>
