<template>
  <q-page padding class="user-background">
    <q-form>
      <div class="q-mb-md">
        <q-input v-model="name" label="Nome" outlined />
      </div>
      <div class="row q-col-gutter-x-md">
        <q-input
          class="col-8"
          mask="##/##/####"
          v-model="bornDate"
          label="Data Nascimento"
          outlined
        >
          <template v-slot:append>
            <q-icon name="event" class="cursor-pointer">
              <q-popup-proxy>
                <q-date v-model="bornDate" mask="DD/MM/YYYY" />
              </q-popup-proxy>
            </q-icon>
          </template>
        </q-input>
        <q-select
          class="col-4"
          v-model="gender"
          label="Genero"
          outlined
          :options="['Masculino', 'Feminino', 'Outro']"
        />
      </div>
      <div class="q-mt-md">
        <password-input label="Senha" outlined v-model="password" @change-icon="onChangeIcon" />
      </div>
      <div class="column">
        <q-checkbox v-model="acceptTerms" label="Aceito os termos de uso" />
        <q-btn label="Salvar" color="primary" @click="onSave" />
      </div>
    </q-form>
  </q-page>
</template>

<script lang="ts">
import { useQuasar } from 'quasar';
import { defineComponent, ref } from 'vue';
import PasswordInput from 'src/components/PasswordInput.vue';

export default defineComponent({
  name: 'UserPage',
  components: {
    PasswordInput,
  },
  setup() {
    const { notify } = useQuasar();

    const name = ref<string>('');
    const bornDate = ref<string>('');
    const gender = ref<string>('');
    const acceptTerms = ref<boolean>(false);
    const password = ref<string>('');

    const onSave = () => {
      notify({
        message: `${name.value} salvo com sucesso`,
        type: 'positive',
      });
    };

    const onChangeIcon = (value: boolean) => {
      if (value) {
        notify({
          message: 'Ocultando senha',
          type: 'info',
        });
      } else {
        notify({
          message: 'Mostrando senha',
          type: 'info',
        });
      }
    };

    return {
      name,
      bornDate,
      gender,
      acceptTerms,
      password,
      onSave,
      onChangeIcon,
    };
  },
});
</script>

<style lang="scss" scoped>
.user-background {
  background-color: white;
}
</style>
