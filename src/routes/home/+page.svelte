<script>
	import account from './account.svelte';
	import { onMount } from 'svelte';
	let cuentas;
	let shadowRoot;
	onMount(() => {
		shadowRoot = cuentas.attachShadow({ mode: "open" });
	    const cuentasComponent = new account({
            target: shadowRoot,
            props: {
                account: document.createElement('div') // Create a container for each account
            }
        });
	});
</script>
<svelte:head>
	<title>Home</title>
	<meta name="Bienvenido" content="Home Page" />
</svelte:head>

<main>
<header>
    <button class="hamburger-btn">
      <span></span>
      <span></span>
      <span></span>
    </button>
  </header>
<div class="text-column">
	<h1>Bienvenido</h1>
	<div>
		<h1>Mis Cuentas</h1>
		<div bind:this={cuentas} class="accounts-container">
		</div>
	
		<div class="operations">
		  <h2>Operaciones</h2>
		  <ul>
			<li>Transferencias</li>
			<li>Pago de Servicios</li>
			<li>Billetera Virtual</li>
			<li>Configuración</li>
			<li>NFC</li>
			<!-- Agregar más opciones de operaciones aquí -->
		  </ul>
		</div>
	</div>
</div>
</main>

<style>
main{
	background: linear-gradient(135deg, #00CED1, #1a439c);
	font-family: Arial, sans-serif;
	
  margin: 0;
  padding: 0;
  widows: 100%;
}
.accounts-container {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
  
}

.account {
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 15px;
  margin: 10px;
  min-width: 200px;
}

.operations {
  text-align: center;
  margin-top: 30px;
}

.operations h2 {
  margin-bottom: 10px;
}

.operations ul {
  list-style: none;
  padding: 0;
  font-weight: bold;
}

.operations li {
  padding: 10px 20px;
  background-color: #f2f2f2;
  border-radius: 5px;
  margin: 5px;
  cursor: pointer;
}

.operations li:hover {
  background-color: #e0e0e0;
}

/* Estilos para el botón tipo hamburguesa */
.hamburger-btn {
  cursor: pointer;
  background-color: transparent;
  border: none;
  padding: 5px;
  margin: 10px;
}

.hamburger-btn span {
  display: block;
  width: 30px;
  height: 3px;
  background-color: #fff;
  margin: 6px 0;
}

@media screen and (max-width: 768px) {
  .accounts-container {
    flex-direction: column;
  }
}
</style>