<template>
  <AdminLayout>
    <Table
      :columns="columns"
      :data="datasource"
      @create="handleCreate"
      @update="handleUpdate"
      @delete="handleDelete"
      :loading="loading"
      :sorted="sort"
      @sort-changed="requestSorted"
    >
      <template v-slot:paginate>
        <Pagination
          :data="datasource"
          :items_per_page="itemsPerPage"
          :current_page="currentPage"
          :total_items="totalItems"
          @page-changed="requestPaginated"
        />
      </template>
    </Table>

    <SidebarForm
      :isOpen="isOpen"
      @closeModal="closeForm"
      :isUpdate="!!currentItem?._id"
      title="serviço"
      @submit="onSubmit"
      :loading="loading"
    >
      <div class="grid gap-4">
        <FormInput
          label="Nome"
          v-bind="form.description"
          :errors="errors.description"
        />

        <FormInput
          label="Descrição"
          v-bind="form.about"
          :errors="errors.about"
        />

        <FormInput
          label="A partir de"
          v-bind="form.price"
          placeholder="00.00"
          :errors="errors.price"
        />

       <FormInput
          label="Tempo (minutos)"
          v-bind="form.time"
          :errors="errors.time"
        />
      </div>
    </SidebarForm>
  </AdminLayout>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { useForm } from "vee-validate";
import { useUserStore } from "@/stores/userStores";
import { required, number } from "@/composable/rules";
import { onlyAdmin } from "@/composable/auth";

const userStore = useUserStore();
const plugin = useNuxtApp();
const config = useRuntimeConfig();
const snackbar = useSnackbar();

const $swal: any = plugin.$swal;

const { token } = userStore;


const api_url = config.public.api_url;
const datasource: any = ref([]);
const currentPage = ref(1);
const itemsPerPage = ref(10);
const totalItems = ref(0);

const sort = ref({
  key: "description",
  order: 1,
});

const loading = ref(false);
const isOpen = ref(false);
const currentItem = ref(null as unknown as any);

const { defineInputBinds, handleSubmit, errors, setValues, resetForm } =
  useForm({
    validationSchema: {
      description: [required],
      about: [],
      price: [required, number],
      time: [required, number],
    },
  });

const state = ref({
  loadValue: {},
});

const form = ref({
  description: defineInputBinds("description"),
  about: defineInputBinds("about"),
  price: defineInputBinds("price"),
  time: defineInputBinds("time"),
});

onMounted(async () => {
  onlyAdmin()
  await requestPagination();
});

const handleCreate = (item: any) => {
  currentItem.value = null;
  isOpen.value = true;
  resetForm();
};

const handleUpdate = async (item: any) => {
  resetForm();
  currentItem.value = item;
  isOpen.value = true;
  await load(currentItem?.value?._id);
};

const handleDelete = (item: any) => {
  remove(item?._id);
};

const closeForm = () => {
  isOpen.value = false;
};

const doAction = async (values: any) => {
  if (!currentItem.value) {
    create(values);
  } else if (currentItem.value._id) {
    update(values);
  }
};

const onSubmit = handleSubmit(doAction);

const columns = [
  {
    key: "description",
    label: "Nome",
    sort: true,
  },
  {
    key: "price",
    label: "Preço",
    type: "double",
    sort: true,
  },
  {
    key: "time",
    label: "Tempo",
    type: "minutes",
    sort: true,
  },
  {
    key: "createdBy",
    label: "Criado por",
    type: "user",
  },
  {
    key: "createdAt",
    label: "Criado em",
    type: "date",
    sort: true,
  },
];

const requestSorted = async (values: any = {}) => {
  await requestPagination({
    ...values,
    paginate: {
      currentPage: currentPage.value,
      itemsPerPage: itemsPerPage.value,
    },
  });
};

const requestPaginated = async (values: any = {}) => {
  await requestPagination({
    ...values,
    sort: {
      key: sort.value.key,
      order: sort.value.order,
    },
  });
};

const requestPagination = async (values: any = {}) => {
  loading.value = true;

  try {
    const response: any = await $fetch(`${api_url}/service/paginate`, {
      method: "GET",
      query: { ...values },
      headers: {
        authorization: `Bearer ${token}`,
      },
    });

    if (response?.services) {
      datasource.value = response.services;
      currentPage.value = response.paginate.currentPage;
      itemsPerPage.value = response.paginate.itemsPerPage;
      totalItems.value = response.paginate.totalItems;
      sort.value.key = response?.sort?.key;
      sort.value.order = response?.sort?.order;
    }
    loading.value = false;
  } catch (error) {
    loading.value = false;
  }
};

const create = async (values: any) => {
  try {
    loading.value = true;
    const response: any = await $fetch(`${api_url}/service`, {
      method: "POST",
      body: { ...values },
      headers: {
        authorization: `Bearer ${token}`,
      },
    });

    if (!response?.service) {
      snackbar.add({
        type: "error",
        text: response.message,
      });
    }

    snackbar.add({
      type: "success",
      text: response?.message,
    });

    closeForm();

    await requestPagination();
    loading.value = false;
  } catch (error) {
    loading.value = false;

    snackbar.add({
      type: "error",
      text: "Ops! Ocorreu um erro...",
    });
  }
};

const update = async (values: any) => {
  try {
    loading.value = true;
    const response: any = await $fetch(
      `${api_url}/service/${currentItem?.value?._id}`,
      {
        method: "PUT",
        body: { ...state.value.loadValue, ...values },
        headers: {
          authorization: `Bearer ${token}`,
        },
      }
    );

    snackbar.add({
      type: "success",
      text: response?.message,
    });

    closeForm();

    await requestPagination();
    loading.value = false;
  } catch (error) {
    loading.value = false;
    snackbar.add({
      type: "error",
      text: "Ops! Ocorreu um erro...",
    });
  }
};

const load = async (_id: string) => {
  try {
    const response: any = await $fetch(`${api_url}/service/${_id}`, {
      method: "GET",
      headers: {
        authorization: `Bearer ${token}`,
      },
    });

    state.value.loadValue = response.service;

    setValues({
      description: response.service?.description,
      price: response.service?.price,
      time: response.service?.time,
      about: response.service?.about,
    });
  } catch (error) {
    closeForm();
    snackbar.add({
      type: "error",
      text: "Ops! Ocorreu um erro...",
    });
  }
};

const remove = async (_id: string) => {
  try {
    $swal
      .fire({
        title: "Tem certeza?",
        text: "Você não poderá reverter isso!",
        icon: "warning",
        showCancelButton: true,
        confirmButtonText: "Sim, remover!",
        cancelButtonText: "Não, cancelar!",
        reverseButtons: true,
        customClass: {
          confirmButton:
            "ml-4 rounded-lg text-white text-bold px-4 py-2 bg-green-500 ",
          cancelButton: "rounded-lg text-white text-bold px-4 py-2 bg-red-500",
        },
        buttonsStyling: false,
      })
      .then(async (result: any) => {
        if (result.isConfirmed) {
          const response: any = await $fetch(`${api_url}/service/${_id}`, {
            method: "DELETE",
            headers: {
              authorization: `Bearer ${token}`,
            },
          });

          snackbar.add({
            type: "success",
            text: response?.message,
          });

          await requestPagination();
        }
      });
  } catch (error) {
    closeForm();
    snackbar.add({
      type: "error",
      text: "Ops! Ocorreu um erro...",
    });
  }
};
</script>