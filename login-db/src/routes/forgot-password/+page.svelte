<script lang="ts">
    import * as Card from '$lib/components/ui/card/index.js';
    import { Input } from '$lib/components/ui/input/index.js';
    import { Label } from '$lib/components/ui/label/index.js';
    import { Button } from '$lib/components/ui/button/index.js';
    import { supabase } from '$lib/supabase.js';

    let email = $state('');
    let loading = $state(false);
    let error = $state('');
    let success = $state(false);

    async function handleSubmit(e: Event) {
        e.preventDefault();
        error = '';
        loading = true;

        const { error: resetError } = await supabase.auth.resetPasswordForEmail(email, {
            redirectTo: `${window.location.origin}/reset-password`
        });

        if (resetError) {
            error = resetError.message;
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
            <Card.Title class="text-2xl font-bold">Forgot password</Card.Title>
            <Card.Description>Enter your email and we'll send you a reset link</Card.Description>
        </Card.Header>
        <Card.Content>
            {#if success}
                <div class="rounded-md bg-green-500/15 p-4 text-sm text-green-600">
                    <p class="font-semibold">Check your email!</p>
                    <p>We've sent you a password reset link.</p>
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

                    <Button type="submit" class="w-full" disabled={loading}>
                        {#if loading}
                            Sending...
                        {:else}
                            Send reset link
                        {/if}
                    </Button>
                </form>
            {/if}
        </Card.Content>
        <Card.Footer class="flex justify-center">
            <p class="text-sm text-muted-foreground">
                Remember your password?
                <a href="/" class="text-primary hover:underline">Sign in</a>
            </p>
        </Card.Footer>
    </Card.Root>
</div>
