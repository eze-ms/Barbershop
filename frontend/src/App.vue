<script setup>
import { useRouter } from "vue-router";
import { useUserStore } from "@/stores/user";

// Instancia de router y estado del usuario
const router = useRouter();
const user = useUserStore();
const currentYear = new Date().getFullYear()


// Redirigir a la página de inicio de sesión
function redirectToLogin() {
  router.push({ name: "login" }); // Solo redirige al login
}
</script>

<template>
  <div class="flex flex-col min-h-screen">
    <!-- Encabezado con información del usuario -->
    <div class="fixed top-0 left-0 w-full z-50 bg-primary shadow">
      <div class="flex justify-between ml-5 mr-5 items-center p-2 max-w-screen-2xl mx-auto">
        <RouterLink :to="{ name: 'home' }">
          <h1 class="text-2xl font-roboto text-white cursor-pointer">Barbershop</h1>
        </RouterLink>

        <div class="flex gap-2 items-center space-x-5">
          <button 
            v-if="!user.user?.name" 
            type="button" 
            class="text-white font-roboto uppercase text-md hover:underline"
            @click="redirectToLogin"
          >
            Iniciar Sesión
          </button>

          <template v-else>
            <p class="text-white text-right font-roboto uppercase text-sm">
              Hola {{ user.getUserName }}
            </p>
            <button 
              type="button" 
              class="text-white font-roboto uppercase text-sm hover:underline"
              @click="user.logout"
            >
              Cerrar Sesión
            </button>
          </template>
        </div>
      </div>
    </div>

<!-- Margen superior para evitar que el contenido quede oculto -->
<div class="mt-20">
  <!-- Resto del contenido de la página -->
</div>


    <!-- Imagen de fondo y navegación -->
    <div class="relative bg-app bg-cover bg-center h-48 sm:h-64 md:h-80 lg:h-96 xl:h-[600px] flex justify-center items-center">
      <div class="absolute inset-0 bg-black opacity-40"></div>

      <!-- Navegación -->
      <nav class="flex gap-10 items-center z-10">
        <!-- Mostrar "Reservar Ahora" si no está autenticado -->
        <template v-if="!user.user?.name">
          <RouterLink
            :to="{ name: 'login' }"
            class="p-3 text-gray-200 font-roboto uppercase text-2xl border-b-2 border-gray-200 transition-transform duration-200 transform hover:scale-110"
          >
            Reservar Ahora
          </RouterLink>
        </template>

        <!-- Mostrar "Mis Citas" y "Nueva Cita" si está autenticado -->
        <template v-else>
          <RouterLink
            :to="{ name: 'my-appointments' }"
            class="p-3 font-roboto text-gray-200 uppercase text-xl border-b-2 border-gray-200 transition-transform duration-200 transform hover:scale-110"
          >
            Mis Citas
          </RouterLink>
          <RouterLink
            :to="{ name: 'new-appointment' }"
            class="p-3 text-gray-200 font-roboto uppercase text-xl border-b-2 border-gray-200 transition-transform duration-200 transform hover:scale-110"
          >
            Nueva Cita
          </RouterLink>
        </template>
      </nav>
    </div>

    <div class="flex-1 bg-white">
      <RouterView />
    </div>

    <!-- Footer -->
    <footer class="bottom-0 left-0 w-full bg-primary">
      <p class="text-center bg-gray-800 text-white text-xs py-4">
        Todos los derechos reservados. {{ currentYear }}
      </p>
    </footer>
  </div>
</template>
