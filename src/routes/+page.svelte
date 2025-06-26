<script>
	import welcome from '$lib/images/svelte-welcome.webp';
	import welcome_fallback from '$lib/images/svelte-welcome.png';
  import { onMount } from 'svelte';
  import CameraModal from './CameraModal.svelte';
  import Icon from '@iconify/svelte';
  let successfoto;
  function openCameraModal() {
    const cameraModal = new CameraModal({
      target: document.body,
    });

    cameraModal.$on("photoTaken", (event) => {
      successfoto = true
      console.log("Photo taken!", event.detail.photoDataUrl);
      // Puedes hacer lo que desees con la foto capturada aquí
      if (successfoto===true) {
        
          setTimeout(() => {
            isLoading = false;
          window.location.href = "/home";
          }, 1000);
        
      }
    });
    
  }
	let isLoading = false;
  let main;
  let buttonFacial;
  let tarjeta = '';
  let tarjetaNumeros = '';
	let username = '';
	let password = '';
  let dynamickeyboard;
	const usernameId = "";
	const passwordId = "";
  let tipoTarjeta = '';

  function detectarTipoTarjeta(numero) {
    if (!numero) return '';
    
    // Eliminar espacios para la validación
    const num = numero.replace(/\s/g, '');
    
    if (/^4/.test(num)) return 'visa';
    if (/^5[1-5]/.test(num)) return 'mastercard';
    if (/^3[47]/.test(num)) return 'amex';
    return '';
  }

  function formatearTarjeta() {
    // Eliminamos espacios en blanco y cualquier carácter que no sea un número
    tarjetaNumeros = tarjetaNumeros.replace(/\D/g, '');
    let tarjetaFormateada = '';
    for (let i = 0; i < tarjetaNumeros.length; i++) {
      if (i > 0 && i % 4 === 0) {
        tarjetaFormateada += ' '; // Agregar espacio cada cuatro números
      }
      tarjetaFormateada += tarjetaNumeros.charAt(i);
    }

    tarjetaNumeros = tarjetaFormateada;
    tipoTarjeta = detectarTipoTarjeta(tarjetaNumeros);
  }

	function handleSubmit() {  
		      
	}

	function handleLogin() {
    if (password.value === '') {
      alert('Ingrese su contraseña');
      return;
    } else{
    isLoading = true;
	let tiempoEspera = Math.floor(Math.random() * 10) + 4;
    setTimeout(() => {
      isLoading = false;
	  window.location.href = "/home";
    }, tiempoEspera*1000);
  }
  }


  function generateRandomNumbers() {
  var numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
  var randomNumbers = [];

    while (numbers.length > 0) {
      var randomIndex = Math.floor(Math.random() * numbers.length);
      randomNumbers.push(numbers[randomIndex]);
      numbers.splice(randomIndex, 1);
    }

    return randomNumbers;
  }
  let shadowRoot;
  function handleKeyboard(){
    var numbers = generateRandomNumbers();
    password.value = "";
    var estilo = `
     <style>
      #keyboard {
  display: block;
  width: 500px;
  position: fixed;
  left: 50%;
  top: 100%;
  transform: translate(-50%, -50%);
  background: #2c3e50;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 8px 24px rgba(0,0,0,0.15);
  z-index: 1000;
}

#overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 999;
}

.keyboard-row {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
}

.key {
  flex: 1;
  min-width: 80px;
  height: 60px;
  background: #ffffff;
  border: none;
  border-radius: 8px;
  font-family: 'Arial', sans-serif;
  font-size: 20px;
  color: #2c3e50;
  cursor: pointer;
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.key:hover {
  background: #ecf0f1;
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

.delete {
  background: #e74c3c;
  color: white;
}

.delete:hover {
  background: #c0392b;
}

       </style>`;

    var htmlString = '<div id="keyboard" style="display: block;">';
    for (var i = 0; i < 3; i++) {
      htmlString += '<div class="keyboard-row">';
      for (var j = 0; j < 3; j++) {
        var index = i * 3 + j;
        htmlString += '<div class="key" id="'+index+'">' + numbers[index] + '</div>';
      }
      htmlString += '</div>';
    }
    htmlString += '<div class="keyboard-row">';
    htmlString += '<div class="key"> </div><div class="key">' + numbers[9] + '</div><div class="key">&lt;</div></div></div>';

    //Validate shadowRoot is null
    if (shadowRoot === undefined || shadowRoot === null) {
      shadowRoot = dynamickeyboard.attachShadow({ mode: "open" });
    }
    const keyboardContainer = document.createElement('div');
    keyboardContainer.innerHTML = estilo + htmlString;
    shadowRoot.appendChild(keyboardContainer);

    keyboardContainer.addEventListener('click', (event) => {
      if (event.target.classList.contains('key')) {
        const clickedKey = event.target.innerText;
        
        // Reflejar el valor de la tecla en el input fuera del Shadow DOM
        const inputOutsideShadow = document.getElementById('password');
        if (clickedKey === '<') {
          inputOutsideShadow.value = inputOutsideShadow.value.slice(0, -1);
          return;
        }
        inputOutsideShadow.value += clickedKey;
        if (inputOutsideShadow.value.length === 6) {
          //close shadowRoot
          shadowRoot.innerHTML = "";
        }
      }
    });
    // Capturar el evento blur (pérdida de enfoque) en keyboardContainer
    main.addEventListener('click', (event) => {
    // Verificar si el clic ocurrió dentro del teclado (Shadow DOM)
      if (event.composedPath().includes(password) || event.composedPath().includes(shadowRoot)) {
        // El clic ocurrió dentro del teclado, no hacemos nada.
        return;
      }
      shadowRoot.innerHTML = '';
    });
    //shadowRoot.innerHTML = estilo + htmlString;
     
  }


  let cuentas;
	let shadowRoot1;
	onMount(() => {
    successfoto=false;
	});

</script>

<svelte:head>
	<title>Responsive Test</title>
	<meta name="Login to Testing" content="QA" />
</svelte:head>

<section bind:this={main} class="mainView">
	<div class="container">
    <h2>Inicio de Sesión</h2>
    <form id="login-form">
      <div class="form-group card-input-wrapper">
        <div class="form-group-row">
          <div class="form-group">
            <div class="card-input-group">
              <input 
                bind:this={tarjeta} 
                bind:value={tarjetaNumeros}
                on:input={formatearTarjeta} 
                placeholder="Tarjeta" 
                type="text" 
                id="tarjeta" 
                name="tarjeta" 
                required 
                maxlength="19"
                class:skeleton={isLoading}
                disabled={isLoading}
              >
              {#if tipoTarjeta}
                <div class="card-logo {tipoTarjeta}"></div>
              {/if}
            </div>
          </div>
        </div>
      </div>
      <div class="form-group-row">
      <div class="form-group">
        <select id="tipo_documento" name="tipo_documento" required class:skeleton={isLoading} disabled={isLoading}>
          <option value="DNI">DNI</option>
          <option value="CE">CE</option>
          <option value="PAS">PAS</option>
        </select>
      </div>
      <div class="form-group">
        <input placeholder="Documento" type="text" id="documento" name="documento" required class:skeleton={isLoading} disabled={isLoading}>
      </div>
    </div>
      <div class="form-group">
        <label for="password">Contraseña o PIN:</label>
        <input bind:this={password}  readOnly="true" type="password" id="password" name="password" on:click={handleKeyboard} required class:skeleton={isLoading} disabled={isLoading}>
        <div id="dynamickeyboard" bind:this={dynamickeyboard}></div>
      </div>
      <div class="form-group">
        <button type="submit" on:click={handleLogin}>Ingresar</button>
      </div>
    </form>
    <button on:click={openCameraModal}>
      <Icon icon="mingcute:face-line" />
      Reconocimiento Facial
    </button>
  </div>  
</section>

<style>
  section {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    flex: 0.6;
  }

  .mainView {
    position: fixed;
    background: linear-gradient(135deg, #1a237e, #0d47a1);
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    text-align: center;
    animation: gradientAnimation 15s ease infinite;
    background-size: 400% 400%;
  }

  @keyframes gradientAnimation {
    0% { background-position: 0% 50%; }
    50% { background-position: 100% 50%; }
    100% { background-position: 0% 50%; }
  }

  * {
    font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
  }

  .container {
    max-width: 400px;
    margin: 2rem auto;
    padding: 2rem;
    border: none;
    border-radius: 12px;
    background-color: rgba(255, 255, 255, 0.95);
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
    backdrop-filter: blur(10px);
  }

  .container h2 {
    text-align: center;
    color: #1a237e;
    font-size: 1.8rem;
    margin-bottom: 2rem;
    font-weight: 600;
  }

  .form-group {
    margin-bottom: 1.5rem;
    display: flex;
    flex-direction: column;
  }

  .form-group label {
    display: block;
    margin-bottom: 0.5rem;
    font-weight: 500;
    color: #424242;
  }

  .form-group-row {
    display: flex;
    gap: 1rem;
  }

  .form-group-row .form-group {
    flex: 1;
  }

  .form-group input,
  .form-group select {
    padding: 0.75rem;
    border: 2px solid #e0e0e0;
    border-radius: 8px;
    font-size: 1rem;
    transition: all 0.3s ease;
  }

  .form-group input:focus,
  .form-group select:focus {
    border-color: #1a237e;
    outline: none;
    box-shadow: 0 0 0 3px rgba(26, 35, 126, 0.1);
  }

  .form-group button,
  button {
    width: 100%;
    padding: 0.875rem;
    background-color: #1a237e;
    border: none;
    color: #fff;
    font-weight: 600;
    font-size: 1rem;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.3s ease;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
  }

  .form-group button:hover,
  button:hover {
    background-color: #0d47a1;
    transform: translateY(-1px);
    box-shadow: 0 4px 12px rgba(26, 35, 126, 0.2);
  }

  .form-group button:active,
  button:active {
    transform: translateY(0);
  }

  @media (max-width: 480px) {
    .container {
      max-width: 100%;
      margin: 1rem;
      padding: 1.5rem;
    }

    .form-group-row {
      flex-direction: column;
      gap: 0.75rem;
    }
  }

.card-input-wrapper {
  width: 100%;
  min-width: 400px; /* Nuevo */
  max-width: 500px; /* Nuevo */
  margin: 0 auto; /* Centrar contenedor */
}

.card-input-group {
  display: flex;
  align-items: center;
  gap: 15px;
  background: white;
  border-radius: 8px;
  padding: 0 15px;
  border: 1px solid #e0e0e0;
  min-width: 80%; /* Nuevo */
  overflow: hidden; /* Nuevo */
}

/* Ajustar input para ocupar tamaño consistente */
.card-input-group input {
  flex: 1 1 auto;
  min-width: 250px;
  border: none;
  outline: none;
  font-size: 16px;
}

@media (max-width: 480px) {
  .card-input-wrapper {
    min-width: 100%;
    max-width: 100%;
  }
  /* Mantener otro estilo móvil existente */
}
  

/* Añadir este nuevo bloque */
.card-input-group input:focus {
  box-shadow: none;
}

.card-input-group:focus-within {
  border-color: #e0e0e0; /* Mantener el color original */
}
.card-logo {
  width: 60px;
  height: 40px;
  background-size: contain;
  background-repeat: no-repeat;
  margin-left: auto;
}

@media (max-width: 480px) {
  .card-input-group {
    gap: 10px;
    padding: 0 10px;
  }
  
  .card-input-group input {
    height: 45px;
    width: 90%;
    font-size: 15px;
  }
  
  .card-logo {
    width: 50px;
    height: 35px;
  }
}

.card-logo.visa {
  background-image: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAxMDAwIDMyNCI+PHBhdGggZmlsbD0iIzE0MzRDQiIgZD0iTTY1MSAySDM5MmwtNTYgMjU5LTI1LTIxNmMtMyAyMi0zNyA0MC02OSA0MGwtMTM3IDEtMS0yaDIzN2wzNS0xNjMgNTUgMjg1IDg2LTI4NSAxMzQgMVYyeiIvPjwvc3ZnPg==');
}

.card-logo.mastercard {
  background-image: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAxMDAwIDc4MCI+PGNpcmNsZSBmaWxsPSIjRUIwMDFCIiBjeD0iNDc1IiBjeT0iMzkwIiByPSIzNzAiLz48Y2lyY2xlIGZpbGw9IiNGNzlFMUIiIGN4PSI1MjUiIGN5PSIzOTAiIHI9IjM3MCIvPjwvc3ZnPg==');
}

.card-logo.amex {
  background-image: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAxMDAwIDEwMDAiPjxwYXRoIGZpbGw9IiMyNjc3QkYiIGQ9Ik0wIDIzMGg1MDB2NTQwSDBWMjMweiIvPjwvc3ZnPg==');
}

.skeleton {
    animation: skeleton-loading 1s linear infinite alternate;
}

@keyframes skeleton-loading {
    0% {
        background-color: hsl(200, 20%, 80%);
    }
    100% {
        background-color: hsl(200, 20%, 95%);
    }
}
</style>
