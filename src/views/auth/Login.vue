<template>
  <section class="flex w-full place-content-center">
    <div class="flex flex-col items-center justify-center text-center mr-28">
      <Logo width="129" height="129" />
      <h3 class="font-sans text-5xl font-bold">fakerface</h3>
      <span class="mt-4 font-sans text-xs">Somos la red social oficial de Cibertec 😎</span>
    </div>
    <div class="flex flex-col items-center justify-center max-w-2xl border-2 border-black rounded-3xl">
      <div class="w-full max-w-md px-5 py-9">
        <form class="space-y-6" @submit.prevent="handleLogin">
          <BaseInput id="email" label="Usuario o correo electrónico" type="text" v-model="user.email" @blur="validateBlur('email')" />
          <BaseInput id="password" label="Contraseña" type="password" v-model="user.password" />
          <Button text="Ingresar" />
        </form>
        <hr class="h-px my-8 bg-black border-0" />
        <div class="flex justify-center w-full">
          <Button :large="false" md-color-background="bg-red-600" @click="redirect('/register')" text="CREAR CUENTA" />
        </div>
      </div>
      <div v-if="v$.user.email.$error">Name field has an error.</div>
    </div>
  </section>
</template>

<script>
import { useVuelidate } from "@vuelidate/core";
import { required, email } from "@vuelidate/validators";
import { mapActions } from "vuex";

import { ref } from "vue";
import { useRouter } from "vue-router";

import BaseInput from "@/components/atoms/BaseInput.vue";
import Button from "@/components/atoms/Button.vue";
import Logo from "@/components/atoms/Logo.vue";

export default {
  name: "Login",
  components: {
    BaseInput,
    Button,
    Logo
  },


  data: () => {
    return {
      user: {
        email: "",
        password: ""
      }
    };
  },
  validations: () => {
    return {
      user: {
        email: { required, email },
        password: { required }
      }
    };
  },
  methods: {
    ...mapActions(['modifyLogged']),
    async validateBlur(data) {
      await this.v$.user[data].$touch;
      const blur = this.v$.user[data].$error;
    },
    handleLogin() {
      try {
        localStorage.setItem("user", JSON.stringify({ status: 200 }));
        this.modifyLogged(true)
        this.$router.push({ name: "Home" });
        window.href
      } catch (error) {
        console.error("errorr", error);
      }
    },

  },
  setup() {
    const username = ref("");
    const password = ref("");

    return {
      username,
      password,
      v$: useVuelidate()
    };
  },
};
</script>
