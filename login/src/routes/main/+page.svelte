<script>
    import { onMount } from 'svelte';
    import { goto } from '$app/navigation';
    import { supabase } from '$lib/supabase.js';
    import '../../app.css';
    import Button from '../../shared/Button.svelte';

    let user = null;

    onMount(async () => {
        const { data: { session } } = await supabase.auth.getSession();
        if (!session) {
            goto('/');
        } else {
            user = session.user;
        }
    });

    const logout = async () => {
        await supabase.auth.signOut();
        goto('/');
    };
</script>

<h1>Welcome</h1>
{#if user}
    <p>You are logged in as {user.email}</p>
    <Button on:click={logout}>Logout</Button>
{/if}
