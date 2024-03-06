<script lang="ts">
    export let label: string;
    export let id: string;
    export let type: 'text' | 'date';
    export let value: string;
    export let required;
    let onInputChange: ((e: Event) => void) | null = null;
    const handleChange = (e: Event) => {
        value = (e.target as HTMLInputElement).value;
    };
</script>
<div class="mb-4">
    <label for={id} class="block text-sm font-medium text-gray-600">{label}:</label>
    {#if type === 'text' || type === 'date'}
        <input {type} {id} bind:value={value} class="border-2 border-grey-300 p-2 w-full" on:input={onInputChange || handleChange}
        />
    {/if}
</div>
