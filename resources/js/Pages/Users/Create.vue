<template>
  <div>
    <Breadcrumbs :items="itemsBreadcrumb" :iconToRender="iconToRender"/>
    <Head title="Crear usuario" />
    <v-card
      class="mx-auto"
      hover
    >
      <v-card-item>
        <v-card-title>
          <h1 class="mb-8 text-3xl font-bold">Usuarios</h1>
        </v-card-title>
      </v-card-item>
      <v-card-text>
        <form @submit.prevent="store">
          <div class="flex flex-wrap -mb-8 -mr-6 p-8">
            <text-input v-model="form.first_name" :error="form.errors.first_name" class="pb-8 pr-6 w-full lg:w-1/2" label="First name" />
            <text-input v-model="form.last_name" :error="form.errors.last_name" class="pb-8 pr-6 w-full lg:w-1/2" label="Last name" />
            <text-input v-model="form.email" :error="form.errors.email" class="pb-8 pr-6 w-full lg:w-1/2" label="Email" />
            <text-input v-model="form.password" :error="form.errors.password" class="pb-8 pr-6 w-full lg:w-1/2" type="password" autocomplete="new-password" label="Password" />
            <select-input v-model="form.owner" :error="form.errors.owner" class="pb-8 pr-6 w-full lg:w-1/2" label="Owner">
              <option :value="true">Yes</option>
              <option :value="false">No</option>
            </select-input>
            <file-input v-model="form.photo" :error="form.errors.photo" class="pb-8 pr-6 w-full lg:w-1/2" type="file" accept="image/*" label="Photo" />
          </div>
          <div class="flex items-center justify-end px-8 py-4 bg-gray-50 border-t border-gray-100">
            <loading-button :loading="form.processing" class="btn-indigo" type="submit">Create User</loading-button>
          </div>
        </form>
      </v-card-text>
    </v-card>
  </div>
</template>

<script>
import { Head, Link } from '@inertiajs/vue3'
import Breadcrumbs from '@/Shared/Layout/Breadcrumbs.vue'
import IconUser from '@/Shared/Icons/User.vue'
import Layout from '@/Shared/Layout.vue'
import FileInput from '@/Shared/FileInput.vue'
import TextInput from '@/Shared/TextInput.vue'
import SelectInput from '@/Shared/SelectInput.vue'
import LoadingButton from '@/Shared/LoadingButton.vue'

export default {
  components: {
    Breadcrumbs,
    IconUser,
    FileInput,
    Head,
    Link,
    LoadingButton,
    SelectInput,
    TextInput,
  },
  layout: Layout,
  remember: 'form',
  data() {
    return {
      form: this.$inertia.form({
        first_name: '',
        last_name: '',
        email: '',
        password: '',
        owner: false,
        photo: null,
      }),
      iconToRender: IconUser,
      itemsBreadcrumb: [
        {
          title: 'Usuarios',
          disabled: false,
          href: baseUrl + '/users',
        },
        {
          title: 'Crear usuario',
          disabled: true,
        },
      ],
    }
  },
  methods: {
    store() {
      this.form.post(baseUrl + '/users')
    },
  },
}
</script>
