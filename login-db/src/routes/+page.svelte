<script lang="ts">
import * as Card from '$lib/components/ui/card/index.js';
import { Input } from '$lib/components/ui/input/index.js';
import { Label } from '$lib/components/ui/label/index.js';
import { Button } from '$lib/components/ui/button/index.js';
import { supabase } from '$lib/supabase.js';
import { goto } from '$app/navigation';

let email = $state('');
let password = $state('');
let loading = $state(false);
let error = $state('');

async function handleSubmit(e: Event) {
    e.preventDefault();
    loading = true;
    error = '';

    const { data, error: authError } = await supabase.auth.signInWithPassword({
        email,
        password
    });

    if (authError) {
        error = authError.message;
    } else {
        goto('/main');
    }
}
</script>

<div class="flex min-h-screen items-center justify-center">
  <Card.Root class="w-full max-w-md">
    <Card.Header class="space-y-1">
      <Card.Title class="text-2xl font-bold">Sign in</Card.Title>
      <Card.Description>Enter your email and password to access your account</Card.Description>
    </Card.Header>
    <Card.Content>
      <form onsubmit={handleSubmit} class="space-y-4">
	{#if error}
	<div class="rounded-md bg-destructive/15 p-3 text-sm text-destructive">
	  {error}
	</div>
	{/if}

	<div class="space-y-2">
	  <Label for="email">Email</Label>
	  <Input
	    id="email"
	    type="email"
	    placeholder="you@example.com"
	    bind:value={email}
	    required
	    />
	</div>

	<div class="space-y-2">
	  <div class="flex items-center justify-between">
	    <Label for="password">Password</Label>
	    <a href="/forgot-password" class="text-sm text-muted-foreground hover:underline">
	      Forgot password?
	    </a>
	  </div>
	  <Input
	    id="password"
	    type="password"
	    placeholder="Enter your password"
	    bind:value={password}
	    required
	    />
	</div>

	<Button type="submit" class="w-full" disabled={loading}>
	  {#if loading}
	  Signing in...
	  {:else}
	  Sign in
	  {/if}
	</Button>
      </form>
    </Card.Content>
    <Card.Footer class="flex justify-center">
      <p class="text-sm text-muted-foreground">
	Don't have an account?
	<a href="/register" class="text-primary hover:underline">Sign up</a>
      </p>
    </Card.Footer>
  </Card.Root>
</div>
