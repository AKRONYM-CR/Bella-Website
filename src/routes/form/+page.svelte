<script>
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation';
	import Swal from 'sweetalert2';

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
    educacion_reconocimientos: [],
		foto_perfil: null,
    instagram: '',
    tiktok: '',
    website: '',
  };

  let loading = false;
  let error = '';
  let newEducacion = '';
  let newReconocimiento = '';

  function agregarEducacionReconocimiento() {
    if (newEducacion && newReconocimiento) {
      form.educacion_reconocimientos = [...form.educacion_reconocimientos, {
        educacion: newEducacion,
        reconocimiento: newReconocimiento
      }];
      newEducacion = '';
      newReconocimiento = '';
    }
  }

  function eliminarEducacionReconocimiento(index) {
    form.educacion_reconocimientos = form.educacion_reconocimientos.filter((_, i) => i !== index);
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
      if (key !== 'foto_perfil' && key !== 'educacion_reconocimientos' && form[key]) {
        data.append(key, form[key]);
      }
    }

    // Manejar foto de perfil
    if (form.foto_perfil) {
      data.append('foto_perfil', form.foto_perfil);
    }

    // Manejar educación y reconocimientos
    if (form.educacion_reconocimientos.length > 0) {
      data.append('educacion_reconocimientos', JSON.stringify(form.educacion_reconocimientos));
    }

    try {
      // Cambia la URL por la de tu backend real
      const res = await fetch('https://bella-backend-testing-6a7ce2262e7e.herokuapp.com/forms', {
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
				educacion_reconocimientos: [],
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
<nav class="bg-[#FDF7FA] shadow-sm px-4">
	<div class="max-w-6xl mx-auto">
		<div class="flex items-center">
			<a href="/">
				<img 
					src="https://bellaApp.b-cdn.net/LOGO%20BELLA-02.png"
					alt="Bella Logo"
					class="h-20"
				/>
			</a>
		</div>
	</div>
</nav>

<div class="flex flex-col bg-[#FDF7FA] items-center min-h-screen py-6">
  <!-- Header Card -->
  <div class="bg-white rounded-xl shadow-md px-6 py-4 mb-6 w-[350px] md:w-[600px] flex flex-col items-center">
    <h2 class="text-2xl font-semibold text-gray-900 mb-1 text-center">Queremos conocerte mejor</h2>
    <p class="text-gray-500 text-sm text-center">Este formulario nos permite entender tu perfil profesional y tu visión como posible aliado inversionista de Bela.</p>
  </div>

  <!-- Form Card -->
  <form class="bg-white rounded-2xl shadow-md px-6 py-8 w-[350px] md:w-[600px] flex flex-col gap-4"
    on:submit|preventDefault={handleSubmit}
    enctype="multipart/form-data"
  >
    <h3 class="text-center text-base font-semibold text-gray-800 mb-2">Información General</h3>
    
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
      <label for="educacion_reconocimientos" class="text-xs text-gray-700 font-medium">Educación y reconocimientos <span class="text-red-500">*</span></label>
      
      <div class="flex gap-2">
        <input 
          placeholder="Educación" 
          class="flex-1 px-4 py-2 text-sm text-gray-900 border border-gray-200 rounded-full focus:outline-none focus:border-gray-400 transition-colors" 
          type="text" 
          bind:value={newEducacion}
        />
        <input 
          placeholder="Reconocimiento" 
          class="flex-1 px-4 py-2 text-sm text-gray-900 border border-gray-200 rounded-full focus:outline-none focus:border-gray-400 transition-colors" 
          type="text" 
          bind:value={newReconocimiento}
        />
        <button 
          type="button"
          class="px-4 py-2 bg-gray-900 text-white text-xs rounded-full hover:bg-gray-800 transition-colors"
          on:click={agregarEducacionReconocimiento}
        >
          Agregar
        </button>
      </div>

      {#if form.educacion_reconocimientos.length > 0}
        <div class="flex flex-col gap-2">
          {#each form.educacion_reconocimientos as item, i}
            <div class="flex items-center gap-2 bg-gray-50 p-3 rounded-lg">
              <div class="flex-1">
                <p class="text-sm font-medium">{item.educacion}</p>
                <p class="text-sm text-gray-500">{item.reconocimiento}</p>
              </div>
              <button 
                type="button"
                class="text-red-500 hover:text-red-700"
                on:click={() => eliminarEducacionReconocimiento(i)}
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

		<button type="submit" class="bg-gray-900 mt-5 text-white px-8 py-4 rounded-full hover:bg-gray-800 transition-colors">
			{loading ? 'Enviando...' : 'Enviar'}
		</button>

  </form>
</div>
