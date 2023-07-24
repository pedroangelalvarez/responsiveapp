<script>
	import welcome from '$lib/images/svelte-welcome.webp';
	import welcome_fallback from '$lib/images/svelte-welcome.png';
	let isLoading = false;
  let main;
  let tarjeta = '';
  let tarjetaNumeros = '';
	let username = '';
	let password = '';
  let dynamickeyboard;
	const usernameId = "";
	const passwordId = "";
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
  display: none;
  margin-top: 1px;
  position: absolute;
  right: 35%; 
  left: 35%;
text-align: center;
  background-color: #fff;
  border: 1px solid #ccc;
}

.keyboard-row {
  display: flex;
}

.key {
  flex: 1;
  height: 40px;
  width: 80px;
  text-align: center;
  border: 1px solid #ccc;
  cursor: pointer;
  line-height: 40px;
}

.delete {
  flex: 2;
}

#overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: none;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 999;
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
    htmlString += '<div class="key"> </div><div class="key">' + numbers[8] + '</div><div class="key">&lt;</div></div></div>';

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

</script>

<svelte:head>
	<title>Responsive Test</title>
	<meta name="Login to Testing" content="QA" />
</svelte:head>

<section bind:this={main} class="mainView">
	<div class="container">
    <h2>Inicio de Sesión</h2>
    <form id="login-form">
      <div class="form-group">
        <input bind:this={tarjeta} bind:value={tarjetaNumeros}
        on:input={formatearTarjeta} placeholder="Tarjeta" type="text" id="tarjeta" name="tarjeta" required maxlength="19">
      </div>
      <div class="form-group-row">
      <div class="form-group">
        <select id="tipo_documento" name="tipo_documento" required>
          <option value="DNI">DNI</option>
          <option value="CE">CE</option>
          <option value="PAS">PAS</option>
        </select>
      </div>
      <div class="form-group">
        <input placeholder="Documento" type="text" id="documento" name="documento" required>
      </div>
    </div>
      <div class="form-group">
        <label for="password">Contraseña o PIN:</label>
        <input bind:this={password}  readOnly="true" type="password" id="password" name="password" on:click={handleKeyboard} required>
        <div id="dynamickeyboard" bind:this={dynamickeyboard}></div>
      </div>
      <div class="form-group">
        <button type="submit" on:click={handleLogin}>Ingresar</button>
      </div>
    </form>
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

	.mainView{
  position: fixed;
  background: linear-gradient(45deg, #3b358f, #6288cf);
  top: 0;
  left: 0;
  height:100%;
  width:100%;
  text-align: center;
  animation: gradientAnimation 10s ease alternate infinite;
}

@keyframes gradientAnimation {
  0% {
    background-position: 0% 50%;
  }
  100% {
    background-position: 100% 50%;
  }
}
* {
      font-family: Arial, sans-serif;
    }

    .container {
      max-width: 400px;
      margin: 0 auto;
      padding: 20px;
      border: 1px solid #ccc;
      border-radius: 5px;
      background-color: #f9f9f9;
    }

    .container h2 {
      text-align: center;
    }

    .form-group {
      margin-bottom: 15px;
  display: flex;
  flex-direction: column;
    }

    .form-group label {
      display: block;
      margin-bottom: 5px;
      font-weight: bold;
    }
    .form-group-row {
  display: flex;
  gap: 10px; /* Adjust the gap between the two form groups */
}

.form-group-row .form-group {
  flex: 1; /* Distribute available space equally to both form groups */
}
  .form-group input {
   
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

  .form-group select {
   
   padding: 8px;
   border: 1px solid #ccc;
   border-radius: 4px;
 }

  .form-group button {
    width: 100%;
    padding: 10px;
    background-color: #4CAF50;
    border: none;
    color: #fff;
    font-weight: bold;
    border-radius: 4px;
    cursor: pointer;
  }

  @media (max-width: 480px) {
    .container {
      max-width: 100%;
      margin: 0 auto;
      padding: 10px;
    }
  }
</style>
