<script>
  import { goto } from '$app/navigation';
  import { supabase } from '$lib/supabase.js';
  import Button from '../shared/Button.svelte';

  let email = "";
  let password = "";
  let error = "";

  const submitHandler = async () => {
      error = "";
      const { data, error: authError } = await supabase.auth.signInWithPassword({
          email,
          password
      });

      if (authError) {
          error = authError.message;
      } else {
          goto('/main');
      }
  };

</script>

<form on:submit|preventDefault={submitHandler}>
  <div class="form-field">
    <label for="email">Email:</label>
    <input type="email" id="email" bind:value={email}>
  </div>
  <div class="form-field">
    <label for="password">Password:</label>
    <input type="password" id="password" bind:value={password}>
  </div>

  {#if error}
    <div class="error">{error}</div>
  {/if}

  <div class="buttons">
    <Button color="link" type="button" on:click={() => goto('/forgot-password')}>I forgot my password</Button>
    <Button color="success" flat={true}>Login</Button>
  </div>

  <div class="signup-link">
    Don't have an account? <a href="/signup">Sign up</a>
  </div>
</form>

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
    color: #d91b42;
    text-align: left;
}

.buttons {
    display: flex;
    justify-content: space-between;
}

.signup-link {
    margin-top: 20px;
    font-size: 14px;
}
</style>
