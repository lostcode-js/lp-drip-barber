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
            src="../public/assets/forgot-password-office01.jpg"
            alt="Office"
          />
        </div>
        <div class="flex items-center justify-center p-6 sm:p-12 md:w-1/2">
          <form @submit="onSubmit">
            <div class="w-full">
              <h1 class="mb-4 text-xl font-semibold text-gray-700">
                Esqueci a senha
              </h1>
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

              <SubmitButton :loading="loading"> Recuperar senha </SubmitButton>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>


<script setup>
import { useUserStore } from "@/stores/userStores";
import { required, email } from "@/composable/rules";
import { useForm } from "vee-validate";

const router = useRouter();
const config = useRuntimeConfig();
const api_url = config.public.api_url;
const snackbar = useSnackbar();

const { defineInputBinds, handleSubmit, errors } = useForm({
  validationSchema: {
    email: [required, email],
  },
});

const loading = ref(false);


const form = ref({
  email: defineInputBinds("email"),
});

const doSubmit = async (values) => {
  try {
    loading.value = true;
    const { error } = await useFetch(
      `${api_url}/recovery-password/${values.email}`,
      {
        method: "POST",
      }
    );

    if (error.value?.data?.message) {
      snackbar.add({
        type: "error",
        text: error.value?.data?.message,
      });
      return;
    }

    snackbar.add({
      type: "success",
      text: "Email de recuperação de senha enviado com sucesso.",
    });

    setTimeout(() => {
      router.push({ path: "/login" });
    }, 1000);
    loading.value = false;
  } catch (error) {
    loading.value = false;

    snackbar.add({
      type: "error",
      text: "Ops! Ocorreu um erro.",
    });
  }
};

const onSubmit = handleSubmit(doSubmit);
</script>


<style scoped>
.max-w-4xl {
  max-width: 56rem;
}
</style>