<template>
  <div>
    <Breadcrumbs :items="itemsBreadcrumb" :iconToRender="iconToRender"/>
    <Head title="Users" />
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
        <div class="flex items-center justify-between mb-6">
          <search-filter v-model="form.search" class="mr-4 w-full max-w-md" @reset="reset">
            <label class="block text-gray-700">Role:</label>
            <select v-model="form.role" class="form-select mt-1 w-full">
              <option :value="null" />
              <option value="user">User</option>
              <option value="owner">Owner</option>
            </select>
            <label class="block mt-4 text-gray-700">Trashed:</label>
            <select v-model="form.trashed" class="form-select mt-1 w-full">
              <option :value="null" />
              <option value="with">With Trashed</option>
              <option value="only">Only Trashed</option>
            </select>
          </search-filter>
          <Link class="btn-indigo" :href="`${baseUrl}/users/create`">
            <span>Crear</span>
            <span class="hidden md:inline">&nbsp;Usuarios</span>
          </Link>
        </div>
        <div class="bg-white rounded-md shadow overflow-x-auto">
          <table class="w-full whitespace-nowrap">
            <tr class="text-left font-bold">
              <th class="pb-4 pt-6 px-6">Name</th>
              <th class="pb-4 pt-6 px-6">Email</th>
              <th class="pb-4 pt-6 px-6" colspan="2">Role</th>
            </tr>
            <tr v-for="user in users" :key="user.id" class="hover:bg-gray-100 focus-within:bg-gray-100">
              <td class="border-t">
                <Link class="flex items-center px-6 py-4 focus:text-indigo-500" :href="`/users/${user.id}/edit`">
                  <img v-if="user.photo" class="block -my-2 mr-2 w-5 h-5 rounded-full" :src="user.photo" />
                  {{ user.name }}
                  <icon v-if="user.deleted_at" name="trash" class="shrink-0 ml-2 w-3 h-3 fill-gray-400" />
                </Link>
              </td>
              <td class="border-t">
                <Link class="flex items-center px-6 py-4" :href="`${baseUrl}/users/${user.id}/edit`" tabindex="-1">
                  {{ user.email }}
                </Link>
              </td>
              <td class="border-t">
                <Link class="flex items-center px-6 py-4" :href="`${baseUrl}/users/${user.id}/edit`" tabindex="-1">
                  {{ user.owner ? 'Owner' : 'User' }}
                </Link>
              </td>
              <td class="w-px border-t">
                <Link class="flex items-center px-4" :href="`${baseUrl}/users/${user.id}/edit`" tabindex="-1">
                  <icon name="cheveron-right" class="block w-6 h-6 fill-gray-400" />
                </Link>
              </td>
            </tr>
            <tr v-if="users.length === 0">
              <td class="px-6 py-4 border-t" colspan="4">No users found.</td>
            </tr>
          </table>
        </div>
      </v-card-text>
    </v-card>
  </div>
</template>

<script>
import { Head, Link } from '@inertiajs/vue3'
import Breadcrumbs from '@/Shared/Layout/Breadcrumbs.vue'
import IconUser from '@/Shared/Icons/User.vue'
import Icon from '@/Shared/Icon.vue'
import pickBy from 'lodash/pickBy'
import Layout from '@/Shared/Layout.vue'
import throttle from 'lodash/throttle'
import mapValues from 'lodash/mapValues'
import SearchFilter from '@/Shared/SearchFilter.vue'

export default {
  components: {
    Breadcrumbs,
    IconUser,
    Head,
    Icon,
    Link,
    SearchFilter,
  },
  layout: Layout,
  props: {
    filters: Object,
    users: Array,
  },
  data() {
    return {
      baseUrl: baseUrl,
      form: {
        search: this.filters.search,
        role: this.filters.role,
        trashed: this.filters.trashed,
      },
      iconToRender: IconUser,
      itemsBreadcrumb: [
         {
          title: 'Usuarios',
          disabled: false,
          href: baseUrl + '/users',
        }
      ],
    }
  },
  watch: {
    form: {
      deep: true,
      handler: throttle(function () {
        this.$inertia.get(`${baseUrl}/users`, pickBy(this.form), { preserveState: true })
      }, 150),
    },
  },
  methods: {
    reset() {
      this.form = mapValues(this.form, () => null)
    },
  },
}
</script>
