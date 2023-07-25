<script>
    import { onMount } from 'svelte';
    let account;
    export let accounts;
    let shadowRoot;
    onMount(() => {
        var numeroRandom = Math.floor(Math.random() * 1000) + 1000;
        shadowRoot = account.attachShadow({ mode: "open" });
	    shadowRoot.innerHTML += `
        <h2>Cuenta Corriente</h2>
			<p>Saldo: $`+numeroRandom+`</p>
            `;
    });
    
    
</script>

<div bind:this={accounts}>
  <div bind:this={account} class="account">
  </div>
</div>

<style>
    .account {
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 15px;
  margin: 10px;
  min-width: 200px;
}
</style>