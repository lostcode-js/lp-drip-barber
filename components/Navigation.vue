<template>
  <header
    class="z-10 w-full py-4 bg-white dark:bg-gray-800 border-b border-gray-200 dark:border-gray-700"
  >
    <div
      class="container flex items-center justify-between h-full mx-auto px-6 text-sky-600 dark:text-sky-300"
    >
      <!-- Mobile hamburger -->
      <button
        v-if="showMenu"
        class="p-1 mr-5 -ml-1 rounded-md md:hidden focus:outline-none focus:shadow-outline-sky"
        v-on:click="toggleSideMenu"
        aria-label="Menu"
      >
        <svg
          class="w-6 h-6"
          aria-hidden="true"
          fill="currentColor"
          viewBox="0 0 20 20"
        >
          <path
            fill-rule="evenodd"
            d="M3 5a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zM3 10a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zM3 15a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1z"
            clip-rule="evenodd"
          ></path>
        </svg>
      </button>
      <!-- Search input -->
      <div class="flex justify-center flex-1 lg:mr-32">
        <div
          class="relative w-full max-w-xl mr-6 focus-within:text-sky-500"
        ></div>
      </div>
      <ul class="flex items-center flex-shrink-0 space-x-6">
        <!-- Theme toggler -->
        <!--
        <li class="flex">
          <button
            class="rounded-md focus:outline-none focus:shadow-outline-sky"
            v-on:click="toggleTheme"
            aria-label="Toggle color mode"
          >
            <template v-if="!dark">
              <svg
                class="w-5 h-5"
                aria-hidden="true"
                fill="currentColor"
                viewBox="0 0 20 20"
              >
                <path
                  d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z"
                ></path>
              </svg>
            </template>
            <template v-if="dark">
              <svg
                class="w-5 h-5"
                aria-hidden="true"
                fill="currentColor"
                viewBox="0 0 20 20"
              >
                <path
                  fill-rule="evenodd"
                  d="M10 2a1 1 0 011 1v1a1 1 0 11-2 0V3a1 1 0 011-1zm4 8a4 4 0 11-8 0 4 4 0 018 0zm-.464 4.95l.707.707a1 1 0 001.414-1.414l-.707-.707a1 1 0 00-1.414 1.414zm2.12-10.607a1 1 0 010 1.414l-.706.707a1 1 0 11-1.414-1.414l.707-.707a1 1 0 011.414 0zM17 11a1 1 0 100-2h-1a1 1 0 100 2h1zm-7 4a1 1 0 011 1v1a1 1 0 11-2 0v-1a1 1 0 011-1zM5.05 6.464A1 1 0 106.465 5.05l-.708-.707a1 1 0 00-1.414 1.414l.707.707zm1.414 8.486l-.707.707a1 1 0 01-1.414-1.414l.707-.707a1 1 0 011.414 1.414zM4 11a1 1 0 100-2H3a1 1 0 000 2h1z"
                  clip-rule="evenodd"
                ></path>
              </svg>
            </template>
          </button>
        </li>
        -->
        <!-- Notifications menu -->
        <li class="relative">
          <button
            v-if="showNotification"
            class="relative align-middle rounded-md focus:outline-none focus:shadow-outline-sky"
            v-on:click="toggleNotificationsMenu"
            @keydown.escape="toggleNotificationsMenu"
            aria-label="Notifications"
            aria-haspopup="true"
          >
            <svg
              class="w-5 h-5"
              aria-hidden="true"
              fill="currentColor"
              viewBox="0 0 20 20"
            >
              <path
                d="M10 2a6 6 0 00-6 6v3.586l-.707.707A1 1 0 004 14h12a1 1 0 00.707-1.707L16 11.586V8a6 6 0 00-6-6zM10 18a3 3 0 01-3-3h6a3 3 0 01-3 3z"
              ></path>
            </svg>
            <!-- Notification badge -->
            <span
              v-if="unreadCount"
              aria-hidden="true"
              class="absolute top-0 right-0 inline-block w-3 h-3 transform translate-x-1 -translate-y-1 bg-red-600 border-2 border-white rounded-full dark:border-gray-800"
            ></span>
          </button>
          <template v-if="isNotificationsMenuOpen">
            <ul
              x-transition:leave="transition ease-in duration-150"
              x-transition:leave-start="opacity-100"
              x-transition:leave-end="opacity-0"
              v-on:click="toggleNotificationsMenu"
              @keydown.escape="toggleNotificationsMenu"
              class="absolute right-0 w-56 p-2 mt-2 space-y-2 text-gray-600 bg-white border border-gray-100 rounded-md shadow-md dark:text-gray-300 dark:border-gray-700 dark:bg-gray-700"
              aria-label="submenu"
            >
              <li class="flex">
                <router-link
                  class="inline-flex items-center justify-between w-full px-2 py-1 text-sm font-semibold transition-colors duration-150 rounded-md hover:bg-gray-100 hover:text-gray-800 dark:hover:bg-gray-800 dark:hover:text-gray-200"
                  :to="`/${route}/notification`"
                  @click="readAllNotification"
                >
                  <span>Notificações</span>
                  <span
                    class="inline-flex items-center justify-center px-2 py-1 text-xs font-bold leading-none text-red-600 bg-red-100 rounded-full dark:text-red-100 dark:bg-red-600"
                  >
                    {{ unreadCount }}
                  </span>
                </router-link>
              </li>
            </ul>
          </template>
        </li>
        <!-- Profile menu -->
        <li class="relative">
          <button
            class="align-middle rounded-full focus:shadow-outline-sky focus:outline-none w-8 h-8 rounded-full"
            v-on:click="toggleProfileMenu"
            @keydown.escape="toggleProfileMenu"
            aria-label="Account"
            aria-haspopup="true"
          >
            <TableAvatar
              :src="userStore.user?.picture"
              :alt="userStore.user?.name"
            />
          </button>
          <template v-if="isProfileMenuOpen">
            <ul
              x-transition:leave="transition ease-in duration-150"
              x-transition:leave-start="opacity-100"
              x-transition:leave-end="opacity-0"
              v-on:click="toggleProfileMenu"
              @keydown.escape="toggleProfileMenu"
              class="absolute right-0 w-56 p-2 mt-2 space-y-2 text-gray-600 bg-white border border-gray-100 rounded-md shadow-md dark:border-gray-700 dark:text-gray-300 dark:bg-gray-700"
              aria-label="submenu"
            >
              <li class="flex">
                <a
                  class="inline-flex items-center w-full px-2 py-1 text-sm font-semibold transition-colors duration-150 rounded-md hover:bg-gray-100 hover:text-gray-800 dark:hover:bg-gray-800 dark:hover:text-gray-200 cursor-pointer"
                  @click="handleUpdate"
                >
                  <svg
                    class="w-4 h-4 mr-3"
                    aria-hidden="true"
                    fill="none"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <path
                      d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"
                    ></path>
                  </svg>
                  <span>Perfil</span>
                </a>
              </li>
              <li class="flex">
                <a
                  class="inline-flex items-center w-full px-2 py-1 text-sm font-semibold transition-colors duration-150 rounded-md hover:bg-gray-100 hover:text-gray-800 dark:hover:bg-gray-800 dark:hover:text-gray-200 cursor-pointer"
                  @click="logout"
                >
                  <svg
                    class="w-4 h-4 mr-3"
                    aria-hidden="true"
                    fill="none"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <path
                      d="M11 16l-4-4m0 0l4-4m-4 4h14m-5 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h7a3 3 0 013 3v1"
                    ></path>
                  </svg>
                  <span>Sair</span>
                </a>
              </li>
            </ul>
          </template>
        </li>
      </ul>
    </div>

    <SidebarForm
      :isOpen="isOpen"
      @closeModal="closeForm"
      :isUpdate="true"
      title="perfil"
      @submit="onSubmit"
    >
      <div>
        <FormAvatar v-model="state.attachment" :src="state.picture" />

        <div class="grid gap-4">
          <FormInput label="Nome" v-bind="form.name" :errors="errors.name" />

          <FormInput label="Email" v-bind="form.email" :errors="errors.email" />

          <FormInput
            v-if="!currentItem"
            label="Password"
            v-bind="form.password"
            type="password"
            :errors="errors.password"
          />

          <FormInput
            label="Telefone"
            v-bind="form.phone"
            :errors="errors.phone"
          />
        </div>
      </div>
    </SidebarForm>
  </header>
</template>

<script setup lang="ts">
import { onMounted, onUnmounted, ref } from "vue";
import { useThemeStore } from "@/stores/themeStores";
import { useUserStore } from "@/stores/userStores";
import { useForm, useFieldArray } from "vee-validate";
import { required, number } from "@/composable/rules";

const emits = defineEmits(["open-menu"]);
const config = useRuntimeConfig();
const api_url = config.public.api_url;

const router = useRouter();
const snackbar = useSnackbar();

const userStore = useUserStore();
const { token } = userStore;

const unreadCount = ref(0);
const isOpen = ref(false);

const props = defineProps({
  route: {
    type: String,
    required: false,
  },
  showMenu: {
    type: String,
    default: true,
  },
  showNotification: {
    type: String,
    default: true,
  }
});

const state = ref({
  picture: "" as unknown as any,
  attachment: "" as unknown as any,
});

const requestPagination = async (values: any = {}) => {
  const response: any = await $fetch(`${api_url}/notification`, {
    method: "GET",
    query: { ...values, readAt: null, user: userStore.user._id },
    headers: {
      authorization: `Bearer ${token}`,
    },
  });

  if (response?.notifications) {
    unreadCount.value = response.notifications?.length ?? 0;
  }
};

const {
  defineInputBinds,
  defineComponentBinds,
  handleSubmit,
  errors,
  setValues,
  resetForm,
} = useForm({
  validationSchema: {
    name: [required],
    email: [required],
    password: [],
    phone: [required],
  },
});

const form = ref({
  name: defineInputBinds("name"),
  email: defineInputBinds("email"),
  phone: defineInputBinds("phone"),
  password: defineInputBinds("password"),
});

const logout = () => {
  userStore.setToken("");
  userStore.setUser({
    _id: "",
    picture: "",
    email: "",
    name: "",
    type: "",
  });
  router.push({ path: "/login" });
};

const uploadAvatar = async () => {
  try {
    if (!state.value.attachment) {
      return;
    }

    let formData = new FormData();
    formData.append("avatar", state.value.attachment);

    const response: any = await $fetch(`${api_url}/avatar`, {
      method: "POST",
      body: formData,
      headers: {
        authorization: `Bearer ${token}`,
      },
    });

    if (!response?.avatar) {
      snackbar.add({
        type: "error",
        text: response.message,
      });
    }

    state.value.picture = response.avatar.url;
  } catch (error) {
    snackbar.add({
      type: "error",
      text: "Ops! Ocorreu um erro ao fazer upload da imagem...",
    });
  }
};

const handleUpdate = async (item: any) => {
  state.value.picture = "";
  state.value.attachment = "";

  setValues({});
  resetForm();
  isOpen.value = true;
  await load();
};

const doAction = async (values: any) => {
  update(values);
};

const onSubmit = handleSubmit(doAction);

const closeForm = () => {
  isOpen.value = false;
};

const readAllNotification = async () => {
  await $fetch(`${api_url}/notification/read-all`, {
    method: "POST",
    headers: {
      authorization: `Bearer ${token}`,
    },
  });

  await requestPagination();
};

const update = async (values: any) => {
  try {
    await uploadAvatar();

    const response: any = await $fetch(
      `${api_url}/user/${userStore?.user?._id}`,
      {
        method: "PUT",
        body: { ...values, ...state.value },
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
  } catch (error) {
    snackbar.add({
      type: "error",
      text: "Ops! Ocorreu um erro...",
    });
  }
};

const load = async () => {
  try {
    const response: any = await $fetch(
      `${api_url}/user/${userStore?.user?._id}`,
      {
        method: "GET",
        headers: {
          authorization: `Bearer ${token}`,
        },
      }
    );

    setValues({
      ...response.user,
      password: "",
    });

    state.value.picture = response.user?.picture ?? "";
  } catch (error) {
    closeForm();
    snackbar.add({
      type: "error",
      text: "Ops! Ocorreu um erro...",
    });
  }
};

const isProfileMenuOpen = ref(false);
const isNotificationsMenuOpen = ref(false);
const isSideMenuOpen = ref(false);
const dark = ref(false);

function toggleTheme() {
  dark.value = !dark.value;
  const themeStores = useThemeStore();
  const lsDark = themeStores.getTheme;
  if (lsDark !== "dark") {
    document.documentElement.classList.add("dark");
    themeStores.setTheme("dark");
  } else {
    document.documentElement.classList.remove("dark");
    themeStores.setTheme("light");
  }
}

const search = ref(null);
const searchItem = ref("");
const shortcut = ref("");

function toggleSideMenu() {
  return emits("open-menu");
}

function toggleProfileMenu() {
  isProfileMenuOpen.value = !isProfileMenuOpen.value;
}

function toggleNotificationsMenu() {
  isNotificationsMenuOpen.value = !isNotificationsMenuOpen.value;
}

function searchShortcut(e: KeyboardEvent) {
  if (e.key === "k" && (e.ctrlKey || e.metaKey)) {
    e.preventDefault();

    // @ts-ignore
    search.value.focus();
  }
}

onMounted(async () => {
  const timeIntervalInMinutes = 10 * 60; //10 min
  await requestPagination();
  setInterval(async () => {
    requestPagination();
  }, timeIntervalInMinutes * 1000); //Convert minutes to milliseconds

  document.addEventListener("keydown", searchShortcut);
});
onUnmounted(() => {
  document.removeEventListener("keydown", searchShortcut);
});
</script>