<template>
  <div>
    <Toast
      v-if="toast"
      :message="toast.message"
      :type="toast.type"
      @close="toast = null"
    />

    <LandingPage
      v-if="currentPage === 'landing'"
      @navigate="handleNavigate"
    />

    <AuthPage
      v-if="currentPage === 'login' || currentPage === 'signup'"
      :is-login="currentPage === 'login'"
      @navigate="handleNavigate"
      @login="handleLogin"
      @show-toast="showToast"
    />

    <Dashboard
      v-if="currentPage === 'dashboard'"
      @navigate="handleNavigate"
      @logout="handleLogout"
      @show-toast="showToast"
    />

    <TicketsPage
      v-if="currentPage === 'tickets'"
      @navigate="handleNavigate"
      @logout="handleLogout"
      @show-toast="showToast"
    />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import LandingPage from './components/LandingPage.vue';
import AuthPage from './components/AuthPage.vue';
import Dashboard from './components/Dashboard.vue';
import TicketsPage from './components/TicketsPage.vue';
import Toast from './components/Toast.vue';

const currentPage = ref('landing');
const isAuthenticated = ref(false);
const toast = ref(null);

onMounted(() => {
  // Initialize demo user
  const users = JSON.parse(localStorage.getItem('ticketapp_users') || '[]');
  if (users.length === 0) {
    localStorage.setItem('ticketapp_users', JSON.stringify([
      { email: 'demo@example.com', password: 'password', name: 'Demo User' }
    ]));
  }

  // Check authentication
  const token = localStorage.getItem('ticketapp_session');
  if (token) {
    isAuthenticated.value = true;
    currentPage.value = 'dashboard';
  }
});

const showToast = (message, type) => {
  toast.value = { message, type };
};

const handleLogin = () => {
  isAuthenticated.value = true;
  currentPage.value = 'dashboard';
};

const handleLogout = () => {
  localStorage.removeItem('ticketapp_session');
  localStorage.removeItem('ticketapp_user');
  isAuthenticated.value = false;
  currentPage.value = 'landing';
};

const handleNavigate = (page) => {
  if (['dashboard', 'tickets'].includes(page) && !isAuthenticated.value) {
    showToast('Please login to access this page', 'error');
    currentPage.value = 'login';
    return;
  }
  currentPage.value = page;
};
</script>