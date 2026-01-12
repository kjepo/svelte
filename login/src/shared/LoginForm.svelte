<script>
  import { goto } from '$app/navigation';
  import Button from '../shared/Button.svelte';

  let username = "Kjell";
  let password = "";
  const errors = { username: "", password: "" };

  const submitHandler = () => {
      console.log(`username: ${username}, password: ${password}`);
      if (username == "Kjell" && password != "123") {
          errors.password = "Incorrect password";
      } else {
          errors.password = "";
          goto('/main');
      }
  };

</script>

<form on:submit|preventDefault={submitHandler}>
  <div class="form-field">
    <label for="username">Username:</label>
    <input type="text" id="username" bind:value={username}>
    <div class="error">{ errors.username }</div>
  </div>
  <div class="form-field">
    <label for="password">Password:</label>
    <input type="password" id="password" bind:value={password}>
    <div class="error">{ errors.password }</div>
  </div>

  <div class="login-btn">
    <Button type="secondary" flat={true}>Login</Button>
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

.login-btn {
    text-align: right;
}
</style>
