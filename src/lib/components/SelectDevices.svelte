<script>
    import { MultiSelect } from 'flowbite-svelte';
    export let selected;
    export let devices;
</script>

<MultiSelect placeholder="Select devices..." items={
    devices.sort((a, b) => a.name.localeCompare(b.name)).map(d => ({value: d.id, ...d}))
    } bind:value={selected} size="lg"
/>
