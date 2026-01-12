<script>
    import { supabase } from '$lib/supabase.js';
    import '../../app.css';
    import Card from '../../shared/Card.svelte';
    import Button from '../../shared/Button.svelte';

    let email = '';
    let password = '';
    let confirmPassword = '';
    let error = '';
    let message = '';

    const handleSubmit = async () => {
        error = '';
        message = '';

        if (password !== confirmPassword) {
            error = 'Passwords do not match.';
            return;
        }

        if (password.length < 6) {
            error = 'Password must be at least 6 characters.';
            return;
        }

        const { data, error: signUpError } = await supabase.auth.signUp({
            email,
            password
        });

        if (signUpError) {
            error = signUpError.message;
        } else {
            message = 'Check your email to confirm your account.';
        }
    };
</script>

<h1>Sign Up</h1>
<Card>
    <form on:submit|preventDefault={handleSubmit}>
        <div class="form-field">
            <label for="email">Email:</label>
            <input type="email" id="email" bind:value={email} required>
        </div>

        <div class="form-field">
            <label for="password">Password:</label>
            <input type="password" id="password" bind:value={password} required>
        </div>

        <div class="form-field">
            <label for="confirmPassword">Confirm Password:</label>
            <input type="password" id="confirmPassword" bind:value={confirmPassword} required>
        </div>

        {#if error}
            <div class="error">{error}</div>
        {/if}

        {#if message}
            <div class="success">{message}</div>
        {/if}

        <div class="buttons">
            <a href="/">Already have an account? Login</a>
            <Button color="primary">Sign Up</Button>
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
    align-items: center;
}

a {
    font-size: 14px;
}
</style>
