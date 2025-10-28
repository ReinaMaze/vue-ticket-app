<template>
  <div class="min-h-screen bg-gray-50">
    <!-- Header -->
    <header class="bg-white shadow-sm">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4">
        <div class="flex justify-between items-center">
          <h1 class="text-2xl font-bold text-blue-600">TicketFlow</h1>
          <button
            @click="$emit('navigate', 'login')"
            class="bg-blue-600 text-white px-6 py-2 rounded-lg hover:bg-blue-700 transition"
          >
            Login
          </button>
        </div>
      </div>
    </header>

    <!-- Hero Section with Wave -->
    <section class="relative bg-gradient-to-br from-blue-600 to-purple-700 text-white overflow-hidden">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-20 relative z-10">
        <div class="text-center">
          <h2 class="text-5xl font-bold mb-6">Manage Your Tickets Effortlessly</h2>
          <p class="text-xl mb-8 text-blue-100">Streamline your workflow with our powerful ticket management system</p>
          <div class="flex gap-4 justify-center flex-wrap">
            <button
              @click="$emit('navigate', 'signup')"
              class="bg-white text-blue-600 px-8 py-3 rounded-lg font-semibold hover:bg-blue-50 transition"
            >
              Get Started
            </button>
            <button
              @click="$emit('navigate', 'login')"
              class="bg-transparent border-2 border-white text-white px-8 py-3 rounded-lg font-semibold hover:bg-white hover:text-blue-600 transition"
            >
              Login
            </button>
          </div>
        </div>
      </div>

      <!-- Decorative Circles -->
      <div class="absolute top-10 right-10 w-32 h-32 bg-white opacity-10 rounded-full"></div>
      <div class="absolute bottom-20 left-10 w-24 h-24 bg-purple-400 opacity-20 rounded-full"></div>

      <!-- Wave SVG -->
      <svg class="absolute bottom-0 left-0 w-full" viewBox="0 0 1440 120" xmlns="http://www.w3.org/2000/svg">
        <path fill="#F9FAFB" d="M0,64L80,69.3C160,75,320,85,480,80C640,75,800,53,960,48C1120,43,1280,53,1360,58.7L1440,64L1440,120L1360,120C1280,120,1120,120,960,120C800,120,640,120,480,120C320,120,160,120,80,120L0,120Z"></path>
      </svg>
    </section>

    <!-- Features Section -->
    <section class="py-20 bg-gray-50">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <h3 class="text-3xl font-bold text-center mb-12 text-gray-800">Why Choose TicketFlow?</h3>
        <div class="grid md:grid-cols-3 gap-8">
          <div class="bg-white p-8 rounded-xl shadow-md hover:shadow-xl transition">
            <div class="w-16 h-16 bg-blue-100 rounded-full flex items-center justify-center mb-4">
              <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="#2563eb" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M2 9a3 3 0 0 1 0 6v2a2 2 0 0 0 2 2h16a2 2 0 0 0 2-2v-2a3 3 0 0 1 0-6V7a2 2 0 0 0-2-2H4a2 2 0 0 0-2 2Z"></path>
                <path d="M13 5v2"></path>
                <path d="M13 17v2"></path>
                <path d="M13 11v2"></path>
              </svg>
            </div>
            <h4 class="text-xl font-bold mb-3 text-gray-800">Easy Ticket Creation</h4>
            <p class="text-gray-600">Create and manage tickets with a simple, intuitive interface</p>
          </div>
          <div class="bg-white p-8 rounded-xl shadow-md hover:shadow-xl transition">
            <div class="w-16 h-16 bg-green-100 rounded-full flex items-center justify-center mb-4">
              <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="#16a34a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <line x1="12" y1="20" x2="12" y2="10"></line>
                <line x1="18" y1="20" x2="18" y2="4"></line>
                <line x1="6" y1="20" x2="6" y2="16"></line>
              </svg>
            </div>
            <h4 class="text-xl font-bold mb-3 text-gray-800">Real-time Analytics</h4>
            <p class="text-gray-600">Track your ticket status with comprehensive dashboards</p>
          </div>
          <div class="bg-white p-8 rounded-xl shadow-md hover:shadow-xl transition">
            <div class="w-16 h-16 bg-purple-100 rounded-full flex items-center justify-center mb-4">
              <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="#7c3aed" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"></path>
                <polyline points="22 4 12 14.01 9 11.01"></polyline>
              </svg>
            </div>
            <h4 class="text-xl font-bold mb-3 text-gray-800">Seamless Workflow</h4>
            <p class="text-gray-600">Streamline your process from creation to resolution</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Footer -->
    <footer class="bg-gray-800 text-white py-8">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center">
        <p>&copy; 2025 TicketFlow. All rights reserved.</p>
      </div>
    </footer>
  </div>
</template>

<script setup>
defineEmits(['navigate']);
</script>