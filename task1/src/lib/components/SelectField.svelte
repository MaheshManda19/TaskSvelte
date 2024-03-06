<script lang="ts">

    export let label: string;
    export let id: string;
    export let options: Record<string, string>;
    export let value: string;
    export let type='select';
    export let required;
</script>
<div class="mb-4">
    <label for={id} class="block text-sm font-medium text-gray-600">{label}:</label>

    <select class="border-2 border-gray-300 p-2 w-full" bind:value={value}>
        <option value="" disabled selected>Select {label}</option>
        {#each Object.keys(options || {}) as option (option)}
            <option value={options[option]}>{options[option]}</option>
        {/each}
    </select>
</div>
