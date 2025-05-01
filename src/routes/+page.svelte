<!-- Form page with Bella welcome screen -->
<script>
  import { goto } from '$app/navigation';
</script>

<!-- Navbar -->
<nav class="bg-[#FDF7FA] shadow-sm px-4 w-full">
  <div class="max-w-6xl mx-auto">
    <div class="flex items-center justify-between py-2">
      <a href="/">
        <img 
          src="https://bellaApp.b-cdn.net/LOGO%20BELLA-02.png"
          alt="Bella Logo"
          class="h-12 sm:h-16"
        />
      </a>
    </div>
  </div>
</nav>

<div class="min-h-[calc(100vh-4rem)] bg-[#FDF7FA] flex items-center justify-center px-4 py-8">
  <div class="max-w-6xl w-full flex flex-col md:flex-row items-center justify-between gap-8 md:gap-12">
    <!-- Phone Image Section -->
    <div class="w-full md:w-1/2 flex justify-center mb-8 md:mb-0">
      <div class="relative w-[180px] sm:w-[240px]">
        <img 
          src="https://bellaApp.b-cdn.net/phone-2.png" 
          alt="Bella App Preview" 
          class="w-full h-auto"
        />
      </div>
    </div>

    <!-- Content Section -->
    <div class="w-full md:w-1/2 space-y-4 text-center md:text-left">
      <h1 class="text-3xl sm:text-4xl md:text-5xl font-bold text-gray-900">
        Bienvenido a Bela
      </h1>
      
      <div class="space-y-3 px-4 sm:px-0">
        <p class="text-base sm:text-lg text-gray-600">
          Estamos construyendo la plataforma <span class="font-semibold">líder de estética médica en Latinoamérica</span>, conectando experiencia médica con tecnología, confianza y propósito.
        </p>
        
        <p class="text-base sm:text-lg text-gray-600">
          Este formulario es el primer paso para explorar una alianza donde tu trayectoria no solo transforma vidas, sino también el <span class="font-semibold">futuro de una industria</span>.
        </p>
      </div>

      <button 
        class="w-full sm:w-auto cursor-pointer bg-gray-900 text-white px-6 sm:px-8 py-3 rounded-full hover:bg-gray-800 transition-colors text-base sm:text-lg"
        onclick={() => goto('/form')}
      >
        Quiero ser parte de esta revolución
      </button>
    </div>
  </div>
</div>

<style>
  /* Add any custom styles if needed */
</style>