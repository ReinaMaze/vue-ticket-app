<template>
  <div class="min-h-screen bg-gray-50">
    <!-- Header -->
    <header class="bg-white shadow-sm">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4">
        <div class="flex justify-between items-center flex-wrap gap-4">
          <h1 class="text-2xl font-bold text-blue-600">TicketFlow</h1>
          <div class="flex items-center gap-4">
            <button
              @click="$emit('navigate', 'dashboard')"
              class="flex items-center gap-2 text-gray-700 hover:text-blue-600 transition"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="m3 9 9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path>
                <polyline points="9 22 9 12 15 12 15 22"></polyline>
              </svg>
              <span class="hidden sm:inline">Dashboard</span>
            </button>
            <button
              @click="handleLogout"
              class="flex items-center gap-2 bg-red-600 text-white px-4 py-2 rounded-lg hover:bg-red-700 transition"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"></path>
                <polyline points="16 17 21 12 16 7"></polyline>
                <line x1="21" y1="12" x2="9" y2="12"></line>
              </svg>
              <span class="hidden sm:inline">Logout</span>
            </button>
          </div>
        </div>
      </div>
    </header>

    <!-- Main Content -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
      <div class="flex justify-between items-center mb-8 flex-wrap gap-4">
        <h2 class="text-3xl font-bold text-gray-800">Ticket Management</h2>
        <button
          @click="showForm = !showForm"
          class="bg-blue-600 text-white px-6 py-3 rounded-lg font-semibold hover:bg-blue-700 transition flex items-center gap-2"
        >
          <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <line x1="12" y1="5" x2="12" y2="19"></line>
            <line x1="5" y1="12" x2="19" y2="12"></line>
          </svg>
          Create Ticket
        </button>
      </div>

      <!-- Form -->
      <div v-if="showForm" class="bg-white p-8 rounded-xl shadow-md mb-8">
        <h3 class="text-xl font-bold text-gray-800 mb-6">
          {{ editingTicket ? 'Edit Ticket' : 'Create New Ticket' }}
        </h3>
        <form @submit.prevent="handleSubmit" class="space-y-4">
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-1">Title *</label>
            <input
              type="text"
              v-model="formData.title"
              class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
            />
            <p v-if="errors.title" class="text-red-500 text-sm mt-1">{{ errors.title }}</p>
          </div>

          <div>
            <label class="block text-sm font-medium text-gray-700 mb-1">Description</label>
            <textarea
              v-model="formData.description"
              class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
              rows="4"
            ></textarea>
          </div>

          <div class="grid md:grid-cols-2 gap-4">
            <div>
              <label class="block text-sm font-medium text-gray-700 mb-1">Status *</label>
              <select
                v-model="formData.status"
                class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
              >
                <option value="open">Open</option>
                <option value="in_progress">In Progress</option>
                <option value="closed">Closed</option>
              </select>
              <p v-if="errors.status" class="text-red-500 text-sm mt-1">{{ errors.status }}</p>
            </div>

            <div>
              <label class="block text-sm font-medium text-gray-700 mb-1">Priority</label>
              <select
                v-model="formData.priority"
                class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
              >
                <option value="low">Low</option>
                <option value="medium">Medium</option>
                <option value="high">High</option>
              </select>
            </div>
          </div>

          <div class="flex gap-4">
            <button
              type="submit"
              class="bg-blue-600 text-white px-6 py-2 rounded-lg font-semibold hover:bg-blue-700 transition"
            >
              {{ editingTicket ? 'Update' : 'Create' }}
            </button>
            <button
              type="button"
              @click="resetForm"
              class="bg-gray-300 text-gray-700 px-6 py-2 rounded-lg font-semibold hover:bg-gray-400 transition"
            >
              Cancel
            </button>
          </div>
        </form>
      </div>

      <!-- Tickets List -->
      <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-6">
        <div v-if="tickets.length === 0" class="col-span-full text-center py-12 bg-white rounded-xl shadow-md">
          <svg class="mx-auto text-gray-400 mb-4" xmlns="http://www.w3.org/2000/svg" width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M2 9a3 3 0 0 1 0 6v2a2 2 0 0 0 2 2h16a2 2 0 0 0 2-2v-2a3 3 0 0 1 0-6V7a2 2 0 0 0-2-2H4a2 2 0 0 0-2 2Z"></path>
            <path d="M13 5v2"></path>
            <path d="M13 17v2"></path>
            <path d="M13 11v2"></path>
          </svg>
          <p class="text-gray-600">No tickets yet. Create your first ticket!</p>
        </div>

        <div
          v-for="ticket in tickets"
          :key="ticket.id"
          class="bg-white p-6 rounded-xl shadow-md hover:shadow-lg transition"
        >
          <div class="flex justify-between items-start mb-4">
            <h3 class="text-lg font-bold text-gray-800 flex-1">{{ ticket.title }}</h3>
            <span :class="['px-3 py-1 rounded-full text-sm font-semibold', getStatusColor(ticket.status)]">
              {{ getStatusLabel(ticket.status) }}
            </span>
          </div>

          <p v-if="ticket.description" class="text-gray-600 mb-4 text-sm">{{ ticket.description }}</p>

          <div class="flex items-center justify-between text-sm text-gray-500 mb-4">
            <span>Priority: {{ ticket.priority }}</span>
            <span>{{ formatDate(ticket.createdAt) }}</span>
          </div>

          <div class="flex gap-2">
            <button
              @click="handleEdit(ticket)"
              class="flex-1 bg-blue-100 text-blue-700 px-4 py-2 rounded-lg hover:bg-blue-200 transition flex items-center justify-center gap-2"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M17 3a2.85 2.83 0 1 1 4 4L7.5 20.5 2 22l1.5-5.5Z"></path>
                <path d="m15 5 4 4"></path>
              </svg>
              Edit
            </button>
            <button
              @click="deleteConfirm = ticket.id"
              class="flex-1 bg-red-100 text-red-700 px-4 py-2 rounded-lg hover:bg-red-200 transition flex items-center justify-center gap-2"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M3 6h18"></path>
                <path d="M19 6v14c0 1-1 2-2 2H7c-1 0-2-1-2-2V6"></path>
                <path d="M8 6V4c0-1 1-2 2-2h4c1 0 2 1 2 2v2"></path>
              </svg>
              Delete
            </button>
          </div>
        </div>
      </div>
    </main>

    <!-- Delete Confirmation Modal -->
    <div
      v-if="deleteConfirm"
      class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 px-4"
    >
      <div class="bg-white rounded-xl p-6 max-w-md w-full">
        <h3 class="text-xl font-bold text-gray-800 mb-4">Confirm Delete</h3>
        <p class="text-gray-600 mb-6">Are you sure you want to delete this ticket? This action cannot be undone.</p>
        <div class="flex gap-4">
          <button
            @click="handleDelete(deleteConfirm)"
            class="flex-1 bg-red-600 text-white px-4 py-2 rounded-lg hover:bg-red-700 transition"
          >
            Delete
          </button>
          <button
            @click="deleteConfirm = null"
            class="flex-1 bg-gray-300 text-gray-700 px-4 py-2 rounded-lg hover:bg-gray-400 transition"
          >
            Cancel
          </button>
        </div>
      </div>
    </div>

    <!-- Footer -->
    <footer class="bg-gray-800 text-white py-8 mt-12">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center">
        <p>&copy; 2025 TicketFlow. All rights reserved.</p>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref, onMounted, reactive } from 'vue';

const emit = defineEmits(['navigate', 'logout', 'show-toast']);

const tickets = ref([]);
const showForm = ref(false);
const editingTicket = ref(null);
const formData = reactive({
  title: '',
  description: '',
  status: 'open',
  priority: 'medium'
});
const errors = reactive({});
const deleteConfirm = ref(null);

onMounted(() => {
  loadTickets();
});

const loadTickets = () => {
  const storedTickets = localStorage.getItem('ticketapp_tickets');
  if (storedTickets) {
    try {
      tickets.value = JSON.parse(storedTickets);
    } catch (e) {
      console.error('Error parsing tickets:', e);
      tickets.value = [];
    }
  }
};

const validateForm = () => {
  const newErrors = {};
  if (!formData.title.trim()) newErrors.title = 'Title is required';
  if (!formData.status) newErrors.status = 'Status is required';
  if (!['open', 'in_progress', 'closed'].includes(formData.status)) {
    newErrors.status = 'Status must be open, in_progress, or closed';
  }
  return newErrors;
};

const handleSubmit = () => {
  // Clear previous errors
  Object.keys(errors).forEach(key => delete errors[key]);

  const newErrors = validateForm();
  if (Object.keys(newErrors).length > 0) {
    Object.assign(errors, newErrors);
    return;
  }

  const ticketData = {
    ...formData,
    id: editingTicket.value?.id || Date.now(),
    createdAt: editingTicket.value?.createdAt || new Date().toISOString()
  };

  let updatedTickets;
  if (editingTicket.value) {
    updatedTickets = tickets.value.map(t => t.id === editingTicket.value.id ? ticketData : t);
    emit('show-toast', 'Ticket updated successfully', 'success');
  } else {
    updatedTickets = [...tickets.value, ticketData];
    emit('show-toast', 'Ticket created successfully', 'success');
  }

  localStorage.setItem('ticketapp_tickets', JSON.stringify(updatedTickets));
  tickets.value = updatedTickets;
  resetForm();
};

const resetForm = () => {
  formData.title = '';
  formData.description = '';
  formData.status = 'open';
  formData.priority = 'medium';
  Object.keys(errors).forEach(key => delete errors[key]);
  showForm.value = false;
  editingTicket.value = null;
};

const handleEdit = (ticket) => {
  editingTicket.value = ticket;
  formData.title = ticket.title;
  formData.description = ticket.description;
  formData.status = ticket.status;
  formData.priority = ticket.priority;
  showForm.value = true;
};

const handleDelete = (id) => {
  const updatedTickets = tickets.value.filter(t => t.id !== id);
  localStorage.setItem('ticketapp_tickets', JSON.stringify(updatedTickets));
  tickets.value = updatedTickets;
  deleteConfirm.value = null;
  emit('show-toast', 'Ticket deleted successfully', 'success');
};

const getStatusColor = (status) => {
  switch (status) {
    case 'open': return 'bg-green-100 text-green-800';
    case 'in_progress': return 'bg-amber-100 text-amber-800';
    case 'closed': return 'bg-gray-100 text-gray-800';
    default: return 'bg-gray-100 text-gray-800';
  }
};

const getStatusLabel = (status) => {
  switch (status) {
    case 'in_progress': return 'In Progress';
    case 'open': return 'Open';
    case 'closed': return 'Closed';
    default: return status.charAt(0).toUpperCase() + status.slice(1);
  }
};

const formatDate = (dateString) => {
  return new Date(dateString).toLocaleDateString();
};

const handleLogout = () => {
  emit('logout');
  emit('show-toast', 'Logged out successfully', 'success');
};
</script>