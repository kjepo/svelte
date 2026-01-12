<script lang="ts">
    import * as Card from '$lib/components/ui/card/index.js';
    import { Input } from '$lib/components/ui/input/index.js';
    import { Label } from '$lib/components/ui/label/index.js';
    import { Button } from '$lib/components/ui/button/index.js';
    import { supabase } from '$lib/supabase.js';
    import { goto } from '$app/navigation';

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

        const { error: updateError } = await supabase.auth.updateUser({
            password
        });

        if (updateError) {
            error = updateError.message;
            loading = false;
            return;
        }

        success = true;
        loading = false;

        // Redirect to main page after 2 seconds
        setTimeout(() => {
            goto('/main');
        }, 2000);
    }
</script>

<div class="flex min-h-screen items-center justify-center">
    <Card.Root class="w-full max-w-md">
        <Card.Header class="space-y-1">
            <Card.Title class="text-2xl font-bold">Reset password</Card.Title>
            <Card.Description>Enter your new password</Card.Description>
        </Card.Header>
        <Card.Content>
            {#if success}
                <div class="rounded-md bg-green-500/15 p-4 text-sm text-green-600">
                    <p class="font-semibold">Password updated!</p>
                    <p>Redirecting you to the app...</p>
                </div>
            {:else}
                <form onsubmit={handleSubmit} class="space-y-4">
                    {#if error}
                        <div class="rounded-md bg-destructive/15 p-3 text-sm text-destructive">
                            {error}
                        </div>
                    {/if}

                    <div class="space-y-2">
                        <Label for="password">New Password</Label>
                        <Input
                            id="password"
                            type="password"
                            placeholder="Enter new password"
                            bind:value={password}
                            required
                        />
                    </div>

                    <div class="space-y-2">
                        <Label for="confirm-password">Confirm Password</Label>
                        <Input
                            id="confirm-password"
                            type="password"
                            placeholder="Confirm new password"
                            bind:value={confirmPassword}
                            required
                        />
                    </div>

                    <Button type="submit" class="w-full" disabled={loading}>
                        {#if loading}
                            Updating...
                        {:else}
                            Update password
                        {/if}
                    </Button>
                </form>
            {/if}
        </Card.Content>
    </Card.Root>
</div>
