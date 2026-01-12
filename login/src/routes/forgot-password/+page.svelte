<script>
    import { supabase } from '$lib/supabase.js';
    import '../../app.css';
    import Card from '../../shared/Card.svelte';
    import Button from '../../shared/Button.svelte';

    let email = '';
    let message = '';
    let error = '';

    const handleSubmit = async () => {
        error = '';
        message = '';

        const { error: resetError } = await supabase.auth.resetPasswordForEmail(email, {
            redirectTo: `${window.location.origin}/reset-password`
        });

        if (resetError) {
            error = resetError.message;
        } else {
            message = 'Check your email for a password reset link.';
        }
    };
</script>

<h1>Forgot Password</h1>
<Card>
    <form on:submit|preventDefault={handleSubmit}>
        <div class="form-field">
            <label for="email">Email:</label>
            <input type="email" id="email" bind:value={email} required>
        </div>

        {#if error}
            <div class="error">{error}</div>
        {/if}

        {#if message}
            <div class="success">{message}</div>
        {/if}

        <div class="buttons">
            <Button color="link" type="button" on:click={() => history.back()}>Back to login</Button>
            <Button color="primary">Send reset link</Button>
        </div>
    </form>
</Card>

<style>
form {
    width: 400px;
    margin: 0 auto;
    text-align: center;
}

.form-field {
    margin: 18px auto;
}

input {
    width: 100%;
    border-radius: 6px;
}

label {
    margin: 10px auto;
    text-align: left;
}

.error {
    font-weight: bold;
    font-size: 12px;
    color: #dc3545;
    text-align: left;
    margin-bottom: 10px;
}

.success {
    font-weight: bold;
    font-size: 14px;
    color: #198754;
    text-align: left;
    margin-bottom: 10px;
}

.buttons {
    display: flex;
    justify-content: space-between;
}
</style>
