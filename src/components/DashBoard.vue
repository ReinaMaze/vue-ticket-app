<template>
  <div class="min-h-screen bg-gray-50">
    <!-- Header -->
    <header class="bg-white shadow-sm">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4">
        <div class="flex justify-between items-center flex-wrap gap-4">
          <h1 class="text-2xl font-bold text-blue-600">TicketFlow</h1>
          <div class="flex items-center gap-4">
            <span class="text-gray-700">Welcome, {{ user.name || 'User' }}</span>
            <button
              @click="handleLogout"
              class="flex items-center gap-2 bg-red-600 text-white px-4 py-2 rounded-lg hover:bg-red-700 transition"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"></path>
                <polyline points="16 17 21 12 16 7"></polyline>
                <line x1="21" y1="12" x2="9" y2="12"></line>
              </svg>
              Logout
            </button>
          </div>
        </div>
      </div>
    </header>

    <!-- Main Content -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
      <h2 class="text-3xl font-bold text-gray-800 mb-8">Dashboard</h2>

      <!-- Stats Grid -->
      <div class="grid sm:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
        <div class="bg-white p-6 rounded-xl shadow-md">
          <div class="flex items-center justify-between">
            <div>
              <p class="text-gray-600 text-sm">Total Tickets</p>
              <p class="text-3xl font-bold text-gray-800">{{ stats.total }}</p>
            </div>
            <div class="w-12 h-12 bg-blue-100 rounded-full flex items-center justify-center">
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="#2563eb" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M2 9a3 3 0 0 1 0 6v2a2 2 0 0 0 2 2h16a2 2 0 0 0 2-2v-2a3 3 0 0 1 0-6V7a2 2 0 0 0-2-2H4a2 2 0 0 0-2 2Z"></path>
                <path d="M13 5v2"></path>
                <path d="M13 17v2"></path>
                <path d="M13 11v2"></path>
              </svg>
            </div>
          </div>
        </div>

        <div class="bg-white p-6 rounded-xl shadow-md">
          <div class="flex items-center justify-between">
            <div>
              <p class="text-gray-600 text-sm">Open</p>
              <p class="text-3xl font-bold text-green-600">{{ stats.open }}</p>
            </div>
            <div class="w-12 h-12 bg-green-100 rounded-full flex items-center justify-center">
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="#16a34a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <circle cx="12" cy="12" r="10"></circle>
                <line x1="12" y1="8" x2="12" y2="12"></line>
                <line x1="12" y1="16" x2="12.01" y2="16"></line>
              </svg>
            </div>
          </div>
        </div>

        <div class="bg-white p-6 rounded-xl shadow-md">
          <div class="flex items-center justify-between">
            <div>
              <p class="text-gray-600 text-sm">In Progress</p>
              <p class="text-3xl font-bold text-amber-600">{{ stats.inProgress }}</p>
            </div>
            <div class="w-12 h-12 bg-amber-100 rounded-full flex items-center justify-center">
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="#d97706" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <line x1="12" y1="20" x2="12" y2="10"></line>
                <line x1="18" y1="20" x2="18" y2="4"></line>
                <line x1="6" y1="20" x2="6" y2="16"></line>
              </svg>
            </div>
          </div>
        </div>

        <div class="bg-white p-6 rounded-xl shadow-md">
          <div class="flex items-center justify-between">
            <div>
              <p class="text-gray-600 text-sm">Closed</p>
              <p class="text-3xl font-bold text-gray-600">{{ stats.closed }}</p>
            </div>
            <div class="w-12 h-12 bg-gray-100 rounded-full flex items-center justify-center">
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="#4b5563" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"></path>
                <polyline points="22 4 12 14.01 9 11.01"></polyline>
              </svg>
            </div>
          </div>
        </div>
      </div>

      <!-- Quick Actions -->
      <div class="bg-white p-8 rounded-xl shadow-md">
        <h3 class="text-xl font-bold text-gray-800 mb-4">Quick Actions</h3>
        <div class="flex gap-4 flex-wrap">
          <button
            @click="$emit('navigate', 'tickets')"
            class="bg-blue-600 text-white px-6 py-3 rounded-lg font-semibold hover:bg-blue-700 transition flex items-center gap-2"
          >
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M2 9a3 3 0 0 1 0 6v2a2 2 0 0 0 2 2h16a2 2 0 0 0 2-2v-2a3 3 0 0 1 0-6V7a2 2 0 0 0-2-2H4a2 2 0 0 0-2 2Z"></path>
              <path d="M13 5v2"></path>
              <path d="M13 17v2"></path>
              <path d="M13 11v2"></path>
            </svg>
            Manage Tickets
          </button>
        </div>
      </div>
    </main>

    <!-- Footer -->
    <footer class="bg-gray-800 text-white py-8 mt-12">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center">
        <p>&copy; 2025 TicketFlow. All rights reserved.</p>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue';

const emit = defineEmits(['navigate', 'logout', 'show-toast']);

const tickets = ref([]);
const user = ref({ name: '', email: '' });

onMounted(() => {
  // Load user data
  const storedUser = localStorage.getItem('ticketapp_user');
  if (storedUser) {
    try {
      user.value = JSON.parse(storedUser);
    } catch (e) {
      console.error('Error parsing user data:', e);
      user.value = { name: 'User', email: '' };
    }
  }

  // Load tickets
  const storedTickets = localStorage.getItem('ticketapp_tickets');
  if (storedTickets) {
    try {
      tickets.value = JSON.parse(storedTickets);
    } catch (e) {
      console.error('Error parsing tickets:', e);
      tickets.value = [];
    }
  }
});

const stats = computed(() => ({
  total: tickets.value.length,
  open: tickets.value.filter(t => t.status === 'open').length,
  inProgress: tickets.value.filter(t => t.status === 'in_progress').length,
  closed: tickets.value.filter(t => t.status === 'closed').length
}));

const handleLogout = () => {
  emit('logout');
  emit('show-toast', 'Logged out successfully', 'success');
};
</script>