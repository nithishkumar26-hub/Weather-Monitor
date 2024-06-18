<script>
// @ts-nocheck

    export let columns;
    export let items;
    // export let dateValuesArray;
</script>

<table>
    <thead>
        <tr>
            {#each columns as column}
                <th class="font-poppins text-sm text-white p-2 font-normal">
                    {column.label}
                </th>
                
            {/each}
        </tr>
    </thead>
    <tbody>
        {#each items as item, index}
            <tr>
                {#each columns as column}
                    
                        <td class="font-poppins text-sm text-white p-2 ">
                            {#if column.key === "name"}
                                <span class="flex justify-items-start">{item.date_epoch}</span>
                            {/if}
                            {#if column.key === "max"}
                                <!-- {(item.main.temp_max -273.15).toFixed(1)} -->
                                {item.day.maxtemp_c + " °C"}
                            {/if}
                            {#if column.key === "min"}
                                <!-- {(item.main.temp_min - 273.15).toFixed(1)} -->
                                {item.day.mintemp_c + " °C"}
                            {/if}
                        </td>
                    
                {/each}
            </tr>
        {/each}
    </tbody>
</table>