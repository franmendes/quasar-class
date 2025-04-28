<template>
  <q-input :outlined="outlined" v-model="value" :type="isPsw ? 'password' : 'text'">
    <template v-slot:append>
      <q-icon :name="isPsw ? 'visibility' : 'visibility_off'" @click="onChangePassword" />
    </template>
  </q-input>
</template>

<script lang="ts">
import { defineComponent, ref, computed } from 'vue';
export default defineComponent({
  name: 'PasswordInput',
  props: {
    modelValue: {
      type: String,
      required: true,
    },
    outlined: Boolean,
  },
  emits: ['update:modelValue', 'changeIcon'],
  setup(props, { emit }) {
    const value = computed({
      get: () => props.modelValue,
      set: (value) => {
        emit('update:modelValue', value);
      },
    });

    const isPsw = ref<boolean>(true);

    const onChangePassword = () => {
      isPsw.value = !isPsw.value;
      emit('changeIcon', isPsw.value);
    };

    return { isPsw, value, onChangePassword };
  },
});
</script>

<style lang="scss" scoped></style>
