 <!-- Form page with Bella welcome screen -->
 <script>
    import { goto } from '$app/navigation';
  </script>
  
  <!-- Navbar -->
  <nav class="bg-[#FDF7FA] shadow-sm px-4">
    <div class="max-w-6xl mx-auto">
      <div class="flex items-center">
        <a href="/">
            <img 
              src="https://bellaApp.b-cdn.net/LOGO%20BELLA-02.png"
              alt="Bella Logo"
              class="h-16"
            />
        </a>
      </div>
    </div>
  </nav>

  <div class="min-h-screen bg-[#FDF7FA] flex items-center justify-center p-4">
    <div class="max-w-6xl w-full flex flex-col md:flex-row items-center justify-between gap-12 py-8">
      <!-- Phone Image Section -->
      <div class="w-full md:w-1/2 flex justify-center">
        <div class="relative w-[300px] md:w-[380px]">
          <img 
            src="https://bellaApp.b-cdn.net/phone-2.png" 
            alt="Bella App Preview" 
            class="w-full h-auto object-contain"
          />
        </div>
      </div>
  
      <!-- Content Section -->
      <div class="w-full md:w-1/2 space-y-4 px-6 md:px-8 py-6">
        <h1 class="text-4xl md:text-5xl font-bold text-gray-900">
          Bienvenido a Bela
        </h1>
        
        <div class="space-y-3">
          <p class="text-gray-600 text-lg">
            Estamos construyendo la plataforma <span class="font-semibold">líder de estética médica en Latinoamérica</span>, conectando experiencia médica con tecnología, confianza y propósito.
          </p>
          
          <p class="text-gray-600 text-lg">
            Este formulario es el primer paso para explorar una alianza donde tu trayectoria no solo transforma vidas, sino también el <span class="font-semibold">futuro de una industria</span>.
          </p>
        </div>
  
        <button class="cursor-pointer bg-gray-900 text-white px-8 py-3 rounded-full hover:bg-gray-800 transition-colors" onclick={() => goto('/form')}>
          Quiero ser parte de esta revolución
        </button>
      </div>
    </div>
  </div>
  
  <style>
    /* Add any custom styles if needed */
  </style>