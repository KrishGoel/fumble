<script>
    import { onMount } from "svelte";
    import auth from "$lib/services/auth";
    import { isAuthenticated, user } from "$lib/stores/auth";

    let auth0Client;

    onMount(async () => {
        auth0Client = await auth.createClient();
        isAuthenticated.set(await auth0Client.isAuthenticated());
        user.set(await auth0Client.getUser());
    });

    function login() {
        auth.loginWithPopup(auth0Client);
    }

    function logout() {
        auth.logout(auth0Client);
    }

    import { page } from '$app/stores';
	export let data;
</script>

<div class="w-10/12 mx-auto">
    <h1 class="text-3xl font-bold">Auth0 with SvelteKit</h1>

    {#if $isAuthenticated}
        <h2>Hey {$user.name}!</h2>
        {#if $user.picture}
            <img src={$user.picture} alt={user.name} />
        {/if}
        <button on:click={logout}>Logout</button>
    {:else}
        <button on:click={login}>Login</button>
    {/if}
</div>