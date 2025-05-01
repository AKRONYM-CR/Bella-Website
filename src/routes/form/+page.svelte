<script>
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation';
	import Swal from 'sweetalert2';

  const host = 'https://bella-backend-testing-6a7ce2262e7e.herokuapp.com'
  // const host = 'http://localhost:3000'

  let form = {
    nombre: '',
    apellido: '',
    edad: '',
		profesion: '',
    correo: '',
    telefono: '',
    pais_ejerce: '',
    ciudad: '',
		direccion_consultorio: '',
    descripcion: '',
    anos_experiencia: '',
    educacion: [],
    reconocimientos: [],
		foto_perfil: null,
    instagram: '',
    tiktok: '',
    website: '',
  };

  let loading = false;
  let error = '';
  let newEducacion = '';
  let newReconocimiento = '';

  function agregarEducacion() {
    if (newEducacion) {
      form.educacion = [...form.educacion, newEducacion];
      newEducacion = '';
    }
  }

  function agregarReconocimiento() {
    if (newReconocimiento) {
      form.reconocimientos = [...form.reconocimientos, newReconocimiento];
      newReconocimiento = '';
    }
  }

  function eliminarEducacion(index) {
    form.educacion = form.educacion.filter((_, i) => i !== index);
  }

  function eliminarReconocimiento(index) {
    form.reconocimientos = form.reconocimientos.filter((_, i) => i !== index);
  }

  // For file input
  function handleFileChange(event) {
    const file = event.target.files[0];
    const maxSize = 5 * 1024 * 1024; // 5MB en bytes

    if (file.size > maxSize) {
      Swal.fire({
        title: 'Error',
        text: 'La imagen no debe superar los 5MB',
        icon: 'error'
      });
      event.target.value = ''; // Limpiar el input
      return;
    }
    
    form.foto_perfil = file;
  }

  async function handleSubmit(e) {
    e.preventDefault();
    loading = true;
    error = '';

    const data = new FormData();
    
    // Manejar campos normales
    for (const key in form) {
      if (key !== 'foto_perfil' && key !== 'educacion' && key !== 'reconocimientos' && form[key]) {
        data.append(key, form[key]);
      }
    }

    // Manejar foto de perfil
    if (form.foto_perfil) {
      data.append('foto_perfil', form.foto_perfil);
    }

    // Manejar educación y reconocimientos
    if (form.educacion.length > 0) {
      data.append('educacion', JSON.stringify(form.educacion));
    }
    if (form.reconocimientos.length > 0) {
      data.append('reconocimientos', JSON.stringify(form.reconocimientos));
    }

    try {
      const res = await fetch(`${host}/forms`, {
        method: 'POST',
        body: data
      });

      if (!res.ok) {
        throw new Error('Error al enviar el formulario');
      }

			Swal.fire({
				title: '¡Gracias!',
				text: 'Tu formulario ha sido enviado correctamente.',
				icon: 'success',
			});
      
			// Limpiar el formulario
			form = {
				nombre: '',
				apellido: '',
				edad: '',
				profesion: '',
				correo: '',
				telefono: '',
				pais_ejerce: '',
				ciudad: '',
				direccion_consultorio: '',
				descripcion: '',
				anos_experiencia: '',
				educacion: [],
				reconocimientos: [],
				foto_perfil: null,
				instagram: '',
				tiktok: '',
				website: '',
			};
    } catch (err) {
      error = err.message;
    } finally {
      loading = false;
    }
  }
</script>

<!-- Navbar -->
<nav class="bg-[#FDF7FA] shadow-sm px-4 w-full">
	<div class="max-w-6xl mx-auto w-full">
		<div class="flex items-center justify-center sm:justify-start">
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

<div class="flex flex-col bg-[#FDF7FA] items-center min-h-screen p-4 sm:p-6">
  <!-- Header Card -->
  <div class="bg-white rounded-xl shadow-md px-4 sm:px-6 py-4 mb-6 w-full max-w-[350px] sm:max-w-[600px] flex flex-col items-center">
    <h2 class="text-xl sm:text-2xl font-semibold text-gray-900 mb-1 text-center">Queremos conocerte mejor</h2>
    <p class="text-gray-500 text-xs sm:text-sm text-center">Este formulario nos permite entender tu perfil profesional y tu visión como posible aliado inversionista de Bela.</p>
  </div>

  <!-- Form Card -->
  <form class="bg-white rounded-2xl shadow-md px-4 sm:px-6 py-6 sm:py-8 w-full max-w-[350px] sm:max-w-[600px] flex flex-col gap-4"
    on:submit|preventDefault={handleSubmit}
    enctype="multipart/form-data"
  >
    <h3 class="text-center text-sm sm:text-base font-semibold text-gray-800 mb-2">Información General</h3>
    
		<div class="flex flex-col gap-2">
      <label for="nombre" class="text-xs text-gray-700 font-medium">Nombre <span class="text-red-500">*</span></label>
      <input id="nombre" class="w-full px-4 py-2 text-sm text-gray-900 border border-gray-200 rounded-full focus:outline-none focus:border-gray-400 transition-colors" type="text" bind:value={form.nombre} required />
    </div>

    <div class="flex flex-col gap-2">
      <label for="apellido" class="text-xs text-gray-700 font-medium">Apellido <span class="text-red-500">*</span></label>
      <input id="apellido" class="w-full px-4 py-2 text-sm text-gray-900 border border-gray-200 rounded-full focus:outline-none focus:border-gray-400 transition-colors" type="text" bind:value={form.apellido} required />
    </div>

    <div class="flex flex-col gap-2">
      <label for="correo" class="text-xs text-gray-700 font-medium">Correo electrónico <span class="text-red-500">*</span></label>
      <input id="correo" class="w-full px-4 py-2 text-sm text-gray-900 border border-gray-200 rounded-full focus:outline-none focus:border-gray-400 transition-colors" type="email" bind:value={form.correo} required />
    </div>

    <div class="flex flex-col gap-2">
      <label for="telefono" class="text-xs text-gray-700 font-medium">Número de teléfono <span class="text-red-500">*</span></label>
      <input id="telefono" class="w-full px-4 py-2 text-sm text-gray-900 border border-gray-200 rounded-full focus:outline-none focus:border-gray-400 transition-colors" type="tel" bind:value={form.telefono} required />
    </div>

		<div class="flex flex-col gap-2">
      <label for="fecha_nacimiento" class="text-xs text-gray-700 font-medium">Edad <span class="text-red-500">*</span></label>
      <input id="edad" class="w-full px-4 py-2 text-sm text-gray-900 border border-gray-200 rounded-full focus:outline-none focus:border-gray-400 transition-colors" type="number" min="0" bind:value={form.edad} required />
    </div>

		<div class="flex flex-col gap-2">
      <label for="profesion" class="text-xs text-gray-700 font-medium">Profesión <span class="text-red-500">*</span></label>
			<input id="profesion" class="w-full px-4 py-2 text-sm text-gray-900 border border-gray-200 rounded-full focus:outline-none focus:border-gray-400 transition-colors" type="text" bind:value={form.profesion} required />
    </div>
    
		<div class="flex flex-col gap-2">
      <label for="anos_experiencia" class="text-xs text-gray-700 font-medium">¿Cuántos años de experiencia tienes? <span class="text-red-500">*</span></label>
      <input id="anos_experiencia" class="w-full px-4 py-2 text-sm text-gray-900 border border-gray-200 rounded-full focus:outline-none focus:border-gray-400 transition-colors" type="number" min="0" bind:value={form.anos_experiencia} required />
    </div>

    <div class="flex flex-col gap-2">
      <label for="pais_ejerce" class="text-xs text-gray-700 font-medium">¿En qué país ejerces principalmente? <span class="text-red-500">*</span></label>
      <input id="pais_ejerce" class="w-full px-4 py-2 text-sm text-gray-900 border border-gray-200 rounded-full focus:outline-none focus:border-gray-400 transition-colors" type="text" bind:value={form.pais_ejerce} required />
    </div>

    <div class="flex flex-col gap-2">
      <label for="ciudad" class="text-xs text-gray-700 font-medium">Ciudad <span class="text-red-500">*</span></label>
      <input id="ciudad" class="w-full px-4 py-2 text-sm text-gray-900 border border-gray-200 rounded-full focus:outline-none focus:border-gray-400 transition-colors" type="text" bind:value={form.ciudad} required />
    </div>

		<div class="flex flex-col gap-4">
      <label for="educacion" class="text-xs text-gray-700 font-medium">Educación <span class="text-red-500">*</span></label>
      
      <div class="flex flex-col sm:flex-row gap-2">
        <input 
          placeholder="Educación" 
          class="flex-1 px-4 py-2 text-sm text-gray-900 border border-gray-200 rounded-full focus:outline-none focus:border-gray-400 transition-colors" 
          type="text" 
          bind:value={newEducacion}
        />
        <button 
          type="button"
          class="px-4 py-2 bg-gray-900 text-white text-xs rounded-full hover:bg-gray-800 transition-colors"
          on:click={agregarEducacion}
        >
          Agregar
        </button>
      </div>

      {#if form.educacion.length > 0}
        <div class="flex flex-col gap-2">
          {#each form.educacion as item, i}
            <div class="flex flex-col sm:flex-row items-start sm:items-center gap-2 bg-gray-50 p-3 rounded-lg">
              <div class="flex-1">
                <p class="text-sm font-medium">{item}</p>
              </div>
              <button 
                type="button"
                class="text-red-500 hover:text-red-700"
                on:click={() => eliminarEducacion(i)}
              >
                Eliminar
              </button>
            </div>
          {/each}
        </div>
      {/if}
    </div>

    <div class="flex flex-col gap-4">
      <label for="reconocimientos" class="text-xs text-gray-700 font-medium">Reconocimientos <span class="text-red-500">*</span></label>
      
      <div class="flex flex-col sm:flex-row gap-2">
        <input 
          placeholder="Reconocimiento" 
          class="flex-1 px-4 py-2 text-sm text-gray-900 border border-gray-200 rounded-full focus:outline-none focus:border-gray-400 transition-colors" 
          type="text" 
          bind:value={newReconocimiento}
        />
        <button 
          type="button"
          class="px-4 py-2 bg-gray-900 text-white text-xs rounded-full hover:bg-gray-800 transition-colors"
          on:click={agregarReconocimiento}
        >
          Agregar
        </button>
      </div>

      {#if form.reconocimientos.length > 0}
        <div class="flex flex-col gap-2">
          {#each form.reconocimientos as item, i}
            <div class="flex flex-col sm:flex-row items-start sm:items-center gap-2 bg-gray-50 p-3 rounded-lg">
              <div class="flex-1">
                <p class="text-sm font-medium">{item}</p>
              </div>
              <button 
                type="button"
                class="text-red-500 hover:text-red-700"
                on:click={() => eliminarReconocimiento(i)}
              >
                Eliminar
              </button>
            </div>
          {/each}
        </div>
      {/if}
    </div>

		<div class="flex flex-col gap-2">
      <label for="direccion_consultorio" class="text-xs text-gray-700 font-medium">Dirección del consultorio <span class="text-red-500">*</span></label>
      <input id="direccion_consultorio" class="w-full px-4 py-2 text-sm text-gray-900 border border-gray-200 rounded-full focus:outline-none focus:border-gray-400 transition-colors" type="text" bind:value={form.direccion_consultorio} required />
    </div>

    <div class="flex flex-col gap-2">
      <label for="descripcion" class="text-xs text-gray-700 font-medium">Descripción detallada de su experiencia en medicina (que resuma bien los años de experiencia, para poder armar la biografía del perfil) <span class="text-red-500">*</span></label>
      <textarea rows="3" id="descripcion" class="w-full px-4 py-2 text-sm text-gray-900 border border-gray-200 rounded-lg focus:outline-none focus:border-gray-400 transition-colors" bind:value={form.descripcion} required></textarea>
    </div>

		<div class="flex flex-col gap-2">
      <label for="foto_perfil" class="text-xs text-gray-700 font-medium">Foto de perfil (máx. 5MB) <span class="text-red-500">*</span></label>
      <input id="foto_perfil" class="w-full px-4 py-2 text-sm text-gray-900 border border-gray-200 rounded-full focus:outline-none focus:border-gray-400 transition-colors" accept="image/*" type="file" on:change={handleFileChange} required />
    </div>

		<div class="flex flex-col gap-2">
      <label for="instagram" class="text-xs text-gray-700 font-medium">Instagram</label>
      <input id="instagram" class="w-full px-4 py-2 text-sm text-gray-900 border border-gray-200 rounded-full focus:outline-none focus:border-gray-400 transition-colors" type="text" bind:value={form.instagram} />
    </div>

		<div class="flex flex-col gap-2">
      <label for="tiktok" class="text-xs text-gray-700 font-medium">Tiktok</label>
      <input id="tiktok" class="w-full px-4 py-2 text-sm text-gray-900 border border-gray-200 rounded-full focus:outline-none focus:border-gray-400 transition-colors" type="text" bind:value={form.tiktok} />
    </div>
		
		<div class="flex flex-col gap-2">
      <label for="website" class="text-xs text-gray-700 font-medium">Sitio web (opcional)</label>
      <input id="website" class="w-full px-4 py-2 text-sm text-gray-900 border border-gray-200 rounded-full focus:outline-none focus:border-gray-400 transition-colors" type="text" bind:value={form.website} />
    </div>

		<button type="submit" class="bg-gray-900 mt-5 text-white px-6 sm:px-8 py-3 sm:py-4 rounded-full hover:bg-gray-800 transition-colors w-full">
			{loading ? 'Enviando...' : 'Enviar'}
		</button>

  </form>
</div>
