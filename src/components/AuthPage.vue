<template>
  <div class="min-h-screen bg-gradient-to-br from-blue-50 to-purple-50 flex items-center justify-center px-4">
    <div class="bg-white rounded-xl shadow-2xl p-8 w-full max-w-md">
      <div class="text-center mb-8">
        <h2 class="text-3xl font-bold text-gray-800">{{ isLogin ? 'Welcome Back' : 'Create Account' }}</h2>
        <p class="text-gray-600 mt-2">{{ isLogin ? 'Login to your account' : 'Sign up for TicketFlow' }}</p>
      </div>

      <form @submit.prevent="handleSubmit" class="space-y-4">
        <div v-if="!isLogin">
          <label class="block text-sm font-medium text-gray-700 mb-1">Name</label>
          <input
            type="text"
            v-model="formData.name"
            class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
          />
          <p v-if="errors.name" class="text-red-500 text-sm mt-1">{{ errors.name }}</p>
        </div>

        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1">Email</label>
          <input
            type="email"
            v-model="formData.email"
            class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
          />
          <p v-if="errors.email" class="text-red-500 text-sm mt-1">{{ errors.email }}</p>
        </div>

        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1">Password</label>
          <input
            type="password"
            v-model="formData.password"
            class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
          />
          <p v-if="errors.password" class="text-red-500 text-sm mt-1">{{ errors.password }}</p>
        </div>

        <button
          type="submit"
          class="w-full bg-blue-600 text-white py-3 rounded-lg font-semibold hover:bg-blue-700 transition"
        >
          {{ isLogin ? 'Login' : 'Sign Up' }}
        </button>
      </form>

      <div class="mt-6 text-center">
        <button
          @click="toggleMode"
          class="text-blue-600 hover:underline"
        >
          {{ isLogin ? "Don't have an account? Sign up" : 'Already have an account? Login' }}
        </button>
      </div>

      <div class="mt-6 text-center">
        <button @click="$emit('navigate', 'landing')" class="text-gray-600 hover:underline text-sm">
          Back to Home
        </button>
      </div>

      <div v-if="isLogin" class="mt-6 p-4 bg-blue-50 rounded-lg text-sm text-gray-700">
        <p class="font-semibold">Demo Credentials:</p>
        <p>Email: demo@example.com</p>
        <p>Password: password</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, watch } from 'vue';

const props = defineProps({
  isLogin: {
    type: Boolean,
    default: true
  }
});

const emit = defineEmits(['navigate', 'login', 'show-toast']);

const localIsLogin = ref(props.isLogin);
const formData = reactive({
  email: '',
  password: '',
  name: ''
});
const errors = reactive({});

watch(() => props.isLogin, (newVal) => {
  localIsLogin.value = newVal;
});

const validateForm = () => {
  const newErrors = {};
  if (!formData.email) newErrors.email = 'Email is required';
  else if (!/\S+@\S+\.\S+/.test(formData.email)) newErrors.email = 'Email is invalid';
  if (!formData.password) newErrors.password = 'Password is required';
  else if (formData.password.length < 6) newErrors.password = 'Password must be at least 6 characters';
  if (!localIsLogin.value && !formData.name) newErrors.name = 'Name is required';
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

  if (localIsLogin.value) {
    const users = JSON.parse(localStorage.getItem('ticketapp_users') || '[]');
    const user = users.find(u => u.email === formData.email && u.password === formData.password);
    if (user) {
      const token = 'token_' + Date.now();
      localStorage.setItem('ticketapp_session', token);
      localStorage.setItem('ticketapp_user', JSON.stringify({ name: user.name, email: user.email }));
      emit('login');
      emit('show-toast', 'Login successful!', 'success');
    } else {
      emit('show-toast', 'Invalid credentials. Try demo@example.com / password', 'error');
    }
  } else {
    const users = JSON.parse(localStorage.getItem('ticketapp_users') || '[]');
    if (users.find(u => u.email === formData.email)) {
      emit('show-toast', 'Email already exists', 'error');
      return;
    }
    users.push({ email: formData.email, password: formData.password, name: formData.name });
    localStorage.setItem('ticketapp_users', JSON.stringify(users));
    emit('show-toast', 'Account created! Please login.', 'success');
    localIsLogin.value = true;
    emit('navigate', 'login');
    formData.email = '';
    formData.password = '';
    formData.name = '';
  }
};

const toggleMode = () => {
  localIsLogin.value = !localIsLogin.value;
  Object.keys(errors).forEach(key => delete errors[key]);
  formData.email = '';
  formData.password = '';
  formData.name = '';
  emit('navigate', localIsLogin.value ? 'login' : 'signup');
};
</script>