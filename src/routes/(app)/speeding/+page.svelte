<script>
    import {Button, Heading, Toolbar, Spinner} from "flowbite-svelte";
    import SelectDevices from "$lib/components/SelectDevices.svelte";
    import DatePicker from "$lib/components/DatePicker.svelte";
    import {setAlert} from "$lib/store.js";
    let loadingReport = false
    let start, end, selected, datePicker
    export let data
    let reportLoaded = false
</script>

<Heading tag="h1" class="text-xl font-semibold text-gray-900 dark:text-white sm:text-2xl">
    Speeding Report
</Heading>
<Toolbar class="w-full py-4 text-gray-500 dark:text-gray-400" embedded>
    <div class="flex gap-4">
        <SelectDevices devices={data.devices} bind:selected="{selected}"/>
        <DatePicker bind:datePicker="{datePicker}"></DatePicker>
    </div>
    <div class="p-4">
        <Button class="whitespace-nowrap" on:click={async () => {
            [start, end] = datePicker.getDates()
            loadingReport = false
            reportLoaded = false
            if (selected && selected.length && start && end) {
                setTimeout(() => loadingReport = true, 100)
            } else {
                setAlert('Please select devices and dates')
            }
        }}>
            {#if loadingReport}
                <Spinner class="me-3" size="4" color="white"/>
            {/if}
            {loadingReport?'Loading...':'Generate'}
        </Button>
    </div>
</Toolbar>

{#if loadingReport || reportLoaded}
    <iframe on:load={() => {
        reportLoaded=true
        loadingReport=false
    }} title="report" class="h-full w-full pb-4" src="{
        `/reports/speeding?start=${new Date(start).toISOString()}&end=${new Date(end).toISOString()}&selected=${selected}`
    }"/>
{/if}
