<script lang="ts">
    export let label: string;
    export let id: string;
    export let options: Record<string, string>;
    export let type: 'radio';
    export let value: string;
    export let required:boolean =false;
</script>
<div class="mb-4">
    <label for={id} class="block text-sm font-medium text-gray-600">{label}:</label>
    <div class="flex">
        {#each Object.keys(options || {}) as option (option)}
            <div class="mr-4">
                <input
                {required}

                    type="radio"
                    id={`${id}-${option}`}
                    bind:group={value}
                    value={options[option]}
                />
                <label for={`${id}-${option}`} class="ml-1">{options[option]}</label>
            </div>
        {/each}
    </div>
</div>