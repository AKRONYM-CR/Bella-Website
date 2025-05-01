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

<div class="thank-you-container">
  <div class="card">
    <h1>Formulario enviado con éxito</h1>
    
    <p class="subtitle">¡Gracias por compartir tu historia y tu visión!</p>

    <img 
      src="https://bellaApp.b-cdn.net/Frame%201171276584.png"
      alt="Email icon"
      class="email-icon"
    />

    <p class="message">
      En Bela creemos en las alianzas que transforman industrias y vidas, estamos felices de que quieras ser parte de esta misión.
    </p>

    <a href="https://bella-backoffice.vercel.app/" class="submit-button">
      Ir a la página de Bela
    </a>
  </div>
</div>

<style>
  .thank-you-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 93vh;
    background-color: #FDF7FA;
  }

  .card {
    background-color: white;
    border-radius: 3rem;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    padding: 3rem 4rem;
    max-width: 700px;
    width: 90%;
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
  }

  h1 {
    font-size: 1.5rem;
    font-weight: 600;
    margin-bottom: 0.5rem;
  }

  .subtitle {
    color: #4B5563;
    font-size: 0.875rem;
    margin-bottom: 1.5rem;
  }

  .email-icon {
    width: 150px;
    height: 150px;
    margin-bottom: 1.5rem;
  }

  .message {
    color: #4B5563;
    font-size: 0.875rem;
    margin-bottom: 1.5rem;
  }

  .submit-button {
    background-color: #1C1B1F;
    color: white;
    width: 100%;
    border-radius: 9999px;
    padding: 0.75rem 1.5rem;
    font-size: 0.875rem;
    font-weight: 500;
    text-decoration: none;
    transition: opacity 0.2s;
  }

  .submit-button:hover {
    opacity: 0.9;
  }
</style>
