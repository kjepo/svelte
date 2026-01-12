<script>
    import { goto } from '$app/navigation';
    import { supabase } from '$lib/supabase.js';
    import '../../app.css';
    import Card from '../../shared/Card.svelte';
    import Button from '../../shared/Button.svelte';

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

        const { error: updateError } = await supabase.auth.updateUser({
            password: password
        });

        if (updateError) {
            error = updateError.message;
        } else {
            message = 'Password updated successfully. Redirecting to login...';
            setTimeout(() => goto('/'), 2000);
        }
    };
</script>

<h1>Reset Password</h1>
<Card>
    <form on:submit|preventDefault={handleSubmit}>
        <div class="form-field">
            <label for="password">New Password:</label>
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
            <Button color="primary">Update Password</Button>
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
    justify-content: flex-end;
}
</style>
