<script lang="ts">
    import { page } from '$app/stores';
    $: errorMessage = $page.error ? $page.error.message : '';
</script>
<div class="min-h-screen flex items-center justify-center bg-blue-600">
    <h1>{$page.status}: {errorMessage}</h1>
</div>
