<template>
  <div class="p-6">
    <h1 class="text-2xl font-bold mb-4">Customers</h1>

    <!-- Add New Customer Form -->
    <form @submit.prevent="submitForm" class="mb-6 space-y-4 bg-gray-100 p-4 rounded-md">
      <div>
        <label class="block font-medium">Name</label>
        <input v-model="form.name" type="text" class="w-full border rounded px-3 py-2" required />
      </div>
      <div>
        <label class="block font-medium">Email</label>
        <input v-model="form.email" type="email" class="w-full border rounded px-3 py-2" required />
      </div>
      <div>
        <label class="block font-medium">Phone</label>
        <input v-model="form.phone" type="text" class="w-full border rounded px-3 py-2" required />
      </div>
      <div>
        <button type="submit" class="bg-blue-600 text-white px-4 py-2 rounded">
          {{ form.id ? 'Update Customer' : 'Add Customer' }}
        </button>
        <button v-if="form.id" type="button" @click="resetForm" class="ml-2 text-gray-600">
          Cancel
        </button>
      </div>
    </form>

    <!-- Customer List -->
    <table class="w-full border">
      <thead class="bg-gray-200">
        <tr>
          <th class="text-left p-2">#</th>
          <th class="text-left p-2">Name</th>
          <th class="text-left p-2">Email</th>
          <th class="text-left p-2">Phone</th>
          <th class="text-left p-2">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(customer, index) in customers" :key="customer.id" class="border-t">
          <td class="p-2">{{ index + 1 }}</td>
          <td class="p-2">{{ customer.name }}</td>
          <td class="p-2">{{ customer.email }}</td>
          <td class="p-2">{{ customer.phone }}</td>
          <td class="p-2">
            <button @click="editCustomer(customer)" class="text-blue-600 hover:underline">Edit</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script setup lang="ts">
import { ref } from 'vue';
import { router, useForm } from '@inertiajs/vue3';

defineProps<{ customers: Array<{ id: number; name: string; email: string; phone: string }> }>()

const form = useForm({
  id: null as number | null,
  name: '',
  email: '',
  phone: '',
});

function submitForm() {
  if (form.id) {
    router.put(`/customers/${form.id}`, form);
  } else {
    router.post('/customers', form);
  }
  resetForm();
}

function editCustomer(customer: typeof form) {
  form.id = customer.id;
  form.name = customer.name;
  form.email = customer.email;
  form.phone = customer.phone;
}

function resetForm() {
  form.id = null;
  form.name = '';
  form.email = '';
  form.phone = '';
}
</script>
