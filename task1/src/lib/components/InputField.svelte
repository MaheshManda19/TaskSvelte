<script lang="ts">
    export let label: string;
    export let id: string;
    export let type: 'text' | 'date';
    export let value: string;
    export let required: boolean = false;
    let onInPutChange:((e:Event)=>void)|null = null;
    const handleChange =(e:Event)=>{
        value =  (e.target as HTMLInputElement).value;
        validateInput();
    }
    const validateInput=()=>{
        const onlyAlpha = /^[A-Za-z]+$/
        if(onlyAlpha.test(value)){
            console.log("Correct Details");
        }
        else{
            console.log('Enter only Alphabets');
        }
    }
</script>
<div class="mb-4">
    <label for={id} class="block text-sm font-medium text-gray-600" >{label}:</label>
    {#if type === 'text' || type === 'date'}
        <input {type} {id} bind:value={value} class="border-2 border-grey-300 p-2 w-full" {required} on:input={onInPutChange || handleChange}
        />
    {/if}
</div>