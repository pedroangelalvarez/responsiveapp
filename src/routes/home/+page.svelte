<script>
	import account from './account.svelte';
	import { beforeUpdate, afterUpdate, onMount } from 'svelte';
	let cuentas;
	let shadowRoot;
	let menuOpen = false;
	
	function toggleMenu() {
		menuOpen = !menuOpen;
	}
	
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
	<title>Banca Digital</title>
	<meta name="Bienvenido" content="Home Page" />
	<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">
</svelte:head>

<main class={menuOpen ? 'menu-open' : ''}>
	<header>
		<div class="logo-container">
			<div class="logo">BD</div>
			<span class="logo-text">Banca Digital</span>
		</div>
		<button class="hamburger-btn" on:click={toggleMenu}>
			<span></span>
			<span></span>
			<span></span>
		</button>
	</header>
	
	<div class="sidebar" class:active={menuOpen}>
		<nav>
			<ul>
				<li><a href="/">Inicio</a></li>
				<li><a href="/">Mis Cuentas</a></li>
				<li><a href="/">Transferencias</a></li>
				<li><a href="/">Pagos</a></li>
				<li><a href="/">Configuración</a></li>
				<li><a hhref="/">Ayuda</a></li>
				<li><a href="/">Salir</a></li>
			</ul>
		</nav>
	</div>
	
	<div class="content-container">
		<div class="welcome-banner">
			<h1>Bienvenido a su Banca Digital</h1>
			<p class="date">{new Date().toLocaleDateString('es-ES', {weekday: 'long', year: 'numeric', month: 'long', day: 'numeric'})}</p>
		</div>
		
		<section class="accounts-section">
			<h2>Mis Cuentas</h2>
			<div bind:this={cuentas} class="accounts-container">
			</div>
		</section>
	
		<section class="operations-section">
			<h2>Operaciones Frecuentes</h2>
			<div class="operations">
				<div class="operation-card">
					<div class="operation-icon">↗</div>
					<span>Transferencias</span>
				</div>
				<div class="operation-card">
					<div class="operation-icon">$</div>
					<span>Pago de Servicios</span>
				</div>
				<div class="operation-card">
					<div class="operation-icon">💳</div>
					<span>Billetera Virtual</span>
				</div>
				<div class="operation-card">
					<div class="operation-icon">⚙</div>
					<span>Configuración</span>
				</div>
				<div class="operation-card">
					<div class="operation-icon">📱</div>
					<span>NFC</span>
				</div>
			</div>
		</section>
	</div>
</main>

<style>
	:global(body) {
		margin: 0;
		padding: 0;
		background-color: #f5f7fa;
	}
	
	main {
		font-family: 'Roboto', sans-serif;
		color: #333;
		position: relative;
		min-height: 100vh;
		transition: all 0.3s ease;
	}
	
	main.menu-open {
		padding-left: 250px;
	}
	
	header {
		background-color: #003366;
		color: white;
		padding: 15px 20px;
		display: flex;
		justify-content: space-between;
		align-items: center;
		box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
		position: sticky;
		top: 0;
		z-index: 100;
	}
	
	.logo-container {
		display: flex;
		align-items: center;
	}
	
	.logo {
		background-color: #fff;
		color: #003366;
		width: 36px;
		height: 36px;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		font-weight: bold;
		font-size: 16px;
		margin-right: 10px;
	}
	
	.logo-text {
		font-size: 18px;
		font-weight: 500;
	}
	
	.sidebar {
		position: fixed;
		left: -250px;
		top: 0;
		width: 250px;
		height: 100%;
		background-color: #002855;
		color: white;
		transition: left 0.3s ease;
		z-index: 200;
		box-shadow: 2px 0 10px rgba(0, 0, 0, 0.1);
		padding-top: 70px;
	}
	
	.sidebar.active {
		left: 0;
	}
	
	.sidebar nav ul {
		list-style: none;
		padding: 0;
		margin: 0;
	}
	
	.sidebar nav li {
		padding: 0;
	}
	
	.sidebar nav a {
		display: block;
		padding: 15px 20px;
		color: white;
		text-decoration: none;
		transition: background-color 0.2s;
		border-left: 4px solid transparent;
	}
	
	.sidebar nav a:hover {
		background-color: rgba(255, 255, 255, 0.1);
		border-left-color: #0077cc;
	}
	
	.hamburger-btn {
		cursor: pointer;
		background-color: transparent;
		border: none;
		padding: 5px;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		width: 30px;
		height: 24px;
	}
	
	.hamburger-btn span {
		display: block;
		width: 100%;
		height: 2px;
		background-color: #fff;
		transition: all 0.3s ease;
	}
	
	.content-container {
		padding: 20px;
		max-width: 1200px;
		margin: 0 auto;
	}
	
	.welcome-banner {
		background: linear-gradient(135deg, #003366, #0077cc);
		color: white;
		padding: 30px;
		border-radius: 10px;
		margin-bottom: 30px;
		box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
	}
	
	.welcome-banner h1 {
		margin: 0 0 10px 0;
		font-size: 24px;
		font-weight: 500;
	}
	
	.date {
		margin: 0;
		opacity: 0.8;
		font-size: 14px;
	}
	
	section {
		background-color: white;
		border-radius: 10px;
		padding: 25px;
		margin-bottom: 30px;
		box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
	}
	
	section h2 {
		margin-top: 0;
		margin-bottom: 20px;
		font-size: 20px;
		font-weight: 500;
		color: #003366;
		border-bottom: 1px solid #eee;
		padding-bottom: 10px;
	}
	
	.accounts-container {
		display: flex;
		justify-content: flex-start;
		flex-wrap: wrap;
		gap: 20px;
	}
	
	.account {
		background: linear-gradient(135deg, #f8f9fa, #e9ecef);
		border: 1px solid #e0e0e0;
		border-radius: 8px;
		padding: 20px;
		min-width: 250px;
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
		transition: transform 0.2s, box-shadow 0.2s;
	}
	
	.account:hover {
		transform: translateY(-5px);
		box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
	}
	
	.operations {
		display: flex;
		flex-wrap: wrap;
		gap: 15px;
		justify-content: flex-start;
	}
	
	.operation-card {
		display: flex;
		flex-direction: column;
		align-items: center;
		padding: 20px 15px;
		background-color: #f8f9fa;
		border-radius: 8px;
		min-width: 120px;
		cursor: pointer;
		transition: all 0.2s ease;
		border: 1px solid #e0e0e0;
	}
	
	.operation-card:hover {
		background-color: #e9ecef;
		transform: translateY(-3px);
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
	}
	
	.operation-icon {
		font-size: 24px;
		margin-bottom: 10px;
		width: 50px;
		height: 50px;
		border-radius: 50%;
		background-color: #e0e0e0;
		display: flex;
		align-items: center;
		justify-content: center;
		color: #003366;
	}
	
	.operation-card span {
		font-size: 14px;
		text-align: center;
		color: #333;
	}
	
	@media screen and (max-width: 768px) {
		main.menu-open {
			padding-left: 0;
		}
		
		.accounts-container {
			flex-direction: column;
		}
		
		.operations {
			justify-content: center;
		}
		
		.welcome-banner {
			padding: 20px;
		}
		
		.welcome-banner h1 {
			font-size: 20px;
		}
	}
</style>