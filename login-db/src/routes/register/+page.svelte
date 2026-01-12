<script lang="ts">
    import * as Card from '$lib/components/ui/card/index.js';
    import { Input } from '$lib/components/ui/input/index.js';
    import { Label } from '$lib/components/ui/label/index.js';
    import { Button } from '$lib/components/ui/button/index.js';
    import { supabase } from '$lib/supabase.js';
    import { goto } from '$app/navigation';

    let email = $state('');
    let password = $state('');
    let confirmPassword = $state('');
    let loading = $state(false);
    let error = $state('');
    let success = $state(false);

    async function handleSubmit(e: Event) {
        e.preventDefault();
        error = '';

        if (password !== confirmPassword) {
            error = 'Passwords do not match';
            return;
        }

        if (password.length < 6) {
            error = 'Password must be at least 6 characters';
            return;
        }

        loading = true;

        const { data, error: authError } = await supabase.auth.signUp({
            email,
            password
        });

        if (authError) {
            error = authError.message;
            loading = false;
            return;
        }

        success = true;
        loading = false;
    }
</script>

<div class="flex min-h-screen items-center justify-center">
    <Card.Root class="w-full max-w-md">
        <Card.Header class="space-y-1">
            <Card.Title class="text-2xl font-bold">Create an account</Card.Title>
            <Card.Description>Enter your email and password to sign up</Card.Description>
        </Card.Header>
        <Card.Content>
            {#if success}
                <div class="rounded-md bg-green-500/15 p-4 text-sm text-green-600">
                    <p class="font-semibold">Check your email!</p>
                    <p>We've sent you a confirmation link to verify your account.</p>
                </div>
            {:else}
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
                        <Label for="password">Password</Label>
                        <Input
                            id="password"
                            type="password"
                            placeholder="Enter your password"
                            bind:value={password}
                            required
                        />
                    </div>

                    <div class="space-y-2">
                        <Label for="confirm-password">Confirm Password</Label>
                        <Input
                            id="confirm-password"
                            type="password"
                            placeholder="Confirm your password"
                            bind:value={confirmPassword}
                            required
                        />
                    </div>

                    <Button type="submit" class="w-full" disabled={loading}>
                        {#if loading}
                            Creating account...
                        {:else}
                            Sign up
                        {/if}
                    </Button>
                </form>
            {/if}
        </Card.Content>
        <Card.Footer class="flex justify-center">
            <p class="text-sm text-muted-foreground">
                Already have an account?
                <a href="/" class="text-primary hover:underline">Sign in</a>
            </p>
        </Card.Footer>
    </Card.Root>
</div>
