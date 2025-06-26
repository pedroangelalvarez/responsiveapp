<script>
    import { onMount } from 'svelte';
    let account;
    export let accounts;
    let shadowRoot;
    let isLoading = true;
    let numeroFormateado;
    
    // Función para actualizar el contenido del shadowRoot
    function updateShadowRoot() {
        if (!shadowRoot) return;
        
        // Actualizar el contenido del balance según el estado de carga
        const balanceElement = shadowRoot.querySelector('.balance-amount');
        if (balanceElement) {
            balanceElement.innerHTML = isLoading ? 
                '<span class="skeleton"></span>' : 
                '$' + numeroFormateado;
        }
    }
    
    onMount(() => {
        // Generar un tiempo aleatorio entre 1 y 5 segundos
        const loadTime = Math.floor(Math.random() * 4000) + 1000;
        
        // Generar el número aleatorio para el balance
        var numeroRandom = Math.floor(Math.random() * 10000) + 5000;
        numeroFormateado = new Intl.NumberFormat('es-ES').format(numeroRandom);
        
        // Inicializar el shadowRoot
        shadowRoot = account.attachShadow({ mode: "open" });
        
        // Simular tiempo de carga
        setTimeout(() => {
            isLoading = false;
            updateShadowRoot(); // Actualizar el DOM cuando termine la carga
        }, loadTime);
        shadowRoot.innerHTML += `
        <style>
            .account-card {
                background: linear-gradient(135deg, #f8f9fa, #e9ecef);
                border: 1px solid #e0e0e0;
                border-radius: 8px;
                padding: 20px;
                min-width: 250px;
                box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
                transition: transform 0.2s, box-shadow 0.2s;
                font-family: 'Roboto', sans-serif;
            }
            .account-card:hover {
                transform: translateY(-5px);
                box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
            }
            .account-type {
                color: #003366;
                font-size: 16px;
                font-weight: 500;
                margin: 0 0 5px 0;
            }
            .account-number {
                color: #666;
                font-size: 12px;
                margin: 0 0 15px 0;
            }
            .balance-label {
                color: #666;
                font-size: 12px;
                margin: 0;
            }
            .balance-amount {
                color: #003366;
                font-size: 22px;
                font-weight: 700;
                margin: 5px 0 0 0;
            }
            .skeleton {
                background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
                background-size: 200% 100%;
                animation: loading 1.5s infinite;
                border-radius: 4px;
                height: 22px;
                width: 120px;
                display: inline-block;
            }
            @keyframes loading {
                0% { background-position: 200% 0; }
                100% { background-position: -200% 0; }
            }
            .account-actions {
                display: flex;
                justify-content: space-between;
                margin-top: 15px;
                padding-top: 15px;
                border-top: 1px solid #e0e0e0;
            }
            .action-btn {
                background: none;
                border: none;
                color: #0077cc;
                font-size: 12px;
                cursor: pointer;
                padding: 0;
            }
            .action-btn:hover {
                text-decoration: underline;
            }
        </style>
        <div class="account-card">
            <h3 class="account-type">Cuenta Corriente</h3>
            <p class="account-number">**** **** **** 4567</p>
            <p class="balance-label">Saldo disponible</p>
            <p class="balance-amount">
                <span class="skeleton"></span>
            </p>
            <div class="account-actions">
                <button class="action-btn">Ver detalle</button>
                <button class="action-btn">Transferir</button>
            </div>
        </div>
        `;
    });
</script>

<div bind:this={accounts}>
  <div bind:this={account}>
  </div>
</div>