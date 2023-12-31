<template>
  <div class="flex items-center min-h-screen p-6 bg-gray-50">
    <div
      class="flex-1 h-full max-w-4xl mx-auto overflow-hidden bg-white rounded-lg shadow-xl"
    >
      <div class="flex flex-col overflow-y-auto md:flex-row">
        <div class="h-32 md:h-auto md:w-1/2">
          <img
            aria-hidden="true"
            class="object-cover w-full h-full"
            src="../public/assets/create-account-office01.jpg"
            alt="Office"
          />
        </div>
        <div class="flex items-center justify-center p-6 sm:p-12 md:w-1/2">
          <form @submit="onSubmit">
            <div class="w-full">
              <h1 class="mb-4 text-xl font-semibold text-gray-700">
                Cadastre-se
              </h1>

              <label class="block text-sm mb-2">
                <span class="text-gray-700">Nome</span>
                <input
                  class="block w-full mt-1 text-sm focus:border-sky-400 focus:outline-none focus:shadow-outline-sky form-input"
                  placeholder="Drip Barber"
                  v-bind="form.name"
                />
                <span class="text-red-600 text-sm mt-2">{{ errors.name }}</span>
              </label>

              <label class="block text-sm">
                <span class="text-gray-700">E-mail</span>
                <input
                  class="block w-full mt-1 text-sm focus:border-sky-400 focus:outline-none focus:shadow-outline-sky form-input"
                  placeholder="example@dripbarber.com"
                  v-bind="form.email"
                />
                <span class="text-red-600 text-sm mt-2">{{
                  errors.email
                }}</span>
              </label>

              <label class="block mt-4 text-sm">
                <span class="text-gray-700">Senha</span>
                <input
                  class="block w-full mt-1 text-sm focus:border-sky-400 focus:outline-none focus:shadow-outline-sky form-input"
                  placeholder="***************"
                  type="password"
                  v-bind="form.password"
                />
                <span class="text-red-600 text-sm mt-2">{{
                  errors.password
                }}</span>
              </label>

              <label class="block mt-4 text-sm">
                <span class="text-gray-700"> Confirmar Senha </span>
                <input
                  class="block w-full mt-1 text-sm focus:border-sky-400 focus:outline-none focus:shadow-outline-sky form-input"
                  placeholder="***************"
                  type="password"
                  v-bind="form.confirmPassword"
                />
                <span class="text-red-600 text-sm mt-2">{{
                  errors.confirmPassword
                }}</span>
              </label>

              <SubmitButton :loading="loading"> Cadastrar </SubmitButton>

              <hr class="my-8" />

              <p class="mt-4">
                <router-link
                  class="text-sm font-medium text-sky-600 hover:underline"
                  to="login"
                >
                  Você já tem uma conta? Login
                </router-link>
              </p>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { useUserStore } from "@/stores/userStores";
import { required, email, password } from "@/composable/rules";
import { useForm } from "vee-validate";

const router = useRouter();
const config = useRuntimeConfig();
const api_url = config.public.api_url;
const snackbar = useSnackbar();

const loading = ref(false);

const { defineInputBinds, handleSubmit, errors } = useForm({
  validationSchema: {
    name: [required],
    email: [required, email],
    password: [required, password],
    confirmPassword: [required, password],
  },
});

const form = ref({
  name: defineInputBinds("name"),
  email: defineInputBinds("email"),
  password: defineInputBinds("password"),
  confirmPassword: defineInputBinds("confirmPassword"),
});

const doSignup = async (values) => {
  try {
    loading.value = true;
    const { error } = await useFetch(`${api_url}/signup`, {
      method: "POST",
      body: { ...values },
    });

    if (error.value?.data?.message) {
      loading.value = false;
      snackbar.add({
        type: "error",
        text: error.value?.data?.message,
      });
      return;
    }

    snackbar.add({
      type: "success",
      text: "Usuário criado com sucesso.",
    });
    loading.value = false;

    setTimeout(() => {
      router.push({ path: "/login" });
    }, 1500);
  } catch (error) {
    loading.value = false;
    snackbar.add({
      type: "error",
      text: "Ops! Ocorreu um erro...",
    });
  }
};

const onSubmit = handleSubmit(doSignup);
</script>


<style scoped>
.max-w-4xl {
  max-width: 56rem;
}
</style>