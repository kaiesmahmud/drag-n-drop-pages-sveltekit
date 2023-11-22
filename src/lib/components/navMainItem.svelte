<script>
    import Icon from '@iconify/svelte';
    export let mainItem ,nodes

    let showDropdown = false ;
    const handleDropDown = ()=>{
        showDropdown = !showDropdown ;
    }
</script>
<div class="bg-white/10 p-2 rounded relative">
    <div class="  flex items-center justify-between gap-10 capitalize">
        <p class="text-lg md:text-xl ">{nodes[mainItem.id]?.name || "Node Not Exist"}</p>
        {#if nodes[mainItem.id]?.items?.length > 0 }
            <button on:click={handleDropDown} class="text-lg lg:text-2xl p-1">
                <Icon icon="ph:dots-three-circle-vertical-fill" />
            </button>
        {/if}
    </div>
    {#if showDropdown}
        <div class=" md:absolute md:top-[100%] mt-3 p-3 left-0 w-full bg-white/10">
            {#if nodes[mainItem.id]?.items?.length > 0 }
                {#each nodes[mainItem.id]?.items as item }
                    <div class="my-2 bg-black/10 p-2">
                        <p>{nodes[item.id]?.name || "Node not Exist"}</p>
                    </div>
                {/each}
            {/if}
        </div>
    {/if}
</div>