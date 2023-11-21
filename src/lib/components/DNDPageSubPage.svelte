<script>
	import { flip } from 'svelte/animate';
	import { dndzone } from 'svelte-dnd-action';
	import Icon from '@iconify/svelte';

	export let nodes 
	export let node,parentId
	
	let inputStyle="p-5 bg-white/10 border-slate-500 focus:border-none w-2/3 rounded"
	const flipDurationMs = 300;
	function handleDndConsider(e) {
		node.items = e.detail.items;
	}
	function handleDndFinalize(e) {
		node.items = e.detail.items;
		nodes = {...nodes};
		console.log(node);
		console.log(nodes);
		// console.log(nodes.node1.items);
	}

	const logme = () =>{
		console.log("log for -",node)
	}
	const deleteNode = () => {
		// console.log("DeleteNode  ",node)
			if (parentId) { 
			// If the node have a parent to catch === 
				console.log("parentId  ",parentId)
				if (parentId == 'home') {
					//If it comes from root parent
					// console.log("Have to Delete from root Parent  ",nodes.node1)
					const items = nodes.node1.items.filter(i=> i.id != node.id)
					// console.log("Remaining",items)
					nodes.node1.items = items;

					//Delete the Node Info from root 
					delete nodes[node.id]
					console.log(nodes)
					
					
				} else {
					//If it comes from root parent
					console.log("Have to Delete from Sub Parent  ",nodes[parentId])

					//Filter and Get Others Childs in a bucket(array)
					const items = nodes[parentId].items.filter(i=> i.id != node.id)
					console.log("Remaining",items)
					nodes[parentId].items = items //Give new Array

					//Delete the Node Info from root 
					delete nodes[node.id]
					console.log(nodes)
				}
			} else {
			// If the node don't have a parent to catch ===
				console.log("parentId Not Found ",parentId)
			}
		
	}
	let viewSubPage = true
	const handleSubPageView = () => {
		viewSubPage = !viewSubPage
		console.log("HandleSubPageView clicked",viewSubPage)
	}

	let needtoAddNewPage = false
	const handleNeedtoAddNewPage = ()=>{
		needtoAddNewPage = !needtoAddNewPage
	}
	let subPageName = ""
	const handleAddSubPages = ()=>{
		//Create a Node Object 
		let  pageName = subPageName
		let  pageURl = subPageName.toLowerCase().replace(/ /g, '-')
		let pageObj = {
			name : pageName, id: pageURl 
		}
		console.log(pageObj)
		subPageName =""
		//Add Node to Parent items 

		nodes[node.id].items.push({id:pageURl})
		//Add Node in main Obj
		nodes = {...nodes,[pageURl]: pageObj}
		console.log(nodes)
}
	let updateName = ""
	let openUpdateSection = false 
	const handleUpdate = () => {
		openUpdateSection = !openUpdateSection
		console.log("got Node to update", node)
		updateName = node.id ;
		
	}
</script>

<div class=" w-full  bg-white/10 rounded  p-2">
	<div class="flex justify-between  p-3 rounded items-center">
		<div>
			<p class="text-xl font-semibold capitalize">{node?.name}</p> 
			<!-- <button on:click={logme} class="bg-teal-500/20 rounded p-2">LogMe</button> -->
		</div>
		<div>
			{#if node?.id != "home"}
				{#if node?.hasOwnProperty("items")}
						{#if node?.items?.length != 0}
							<button on:click={handleSubPageView} class="p-2 rounded bg-slate-800 text-2xl">
								{#if viewSubPage}
								<Icon icon="mdi:hide" />
								{:else}
								<Icon icon="mdi:eye" />
								{/if}
							</button>
						{/if}
				<button on:click={handleNeedtoAddNewPage} class={` p-2 rounded ${needtoAddNewPage?'bg-red-200 text-red-500 ':" bg-green-200 text-green-800 "} text-2xl`}>
				{#if needtoAddNewPage}
					<Icon icon="icomoon-free:cross" />
				{:else}
					<Icon icon="carbon:add-filled" />
				{/if}
				</button>
				{/if}
			
			<button on:click={handleUpdate} class="text-cyan-700 p-2 rounded bg-cyan-200 text-2xl">
				<Icon icon="ic:twotone-drive-file-rename-outline" />
			</button>
			<button on:click={deleteNode} class="text-red-500 p-2 rounded bg-red-200 text-2xl">
				<Icon icon="material-symbols:delete" />
			</button>
			{/if}
			<!-- <div class="text-red-500 p-2 rounded bg-red-200 text-2xl">
				<Icon icon="ph:dots-three-circle-vertical-bold" />
			</div> -->
		</div>
	</div>
	
	{#if node?.hasOwnProperty("items")}
		{#if needtoAddNewPage}
			<div class="m-2 p-1 flex items-center gap-1">
				<input bind:value={subPageName} class={inputStyle} placeholder="Add Sub-Page Name" />
				<button on:click={handleAddSubPages} class="text-green-800 p-5 rounded bg-green-200 text-2xl flex ">
					<Icon icon="carbon:add-filled" />
					<p class="text-sm">Add Page</p>
					
				</button>
			</div>
		{/if}
	{/if}
	<!-- ======= Update Page Name ========== -->
	{#if openUpdateSection}
		<div class="">
			<input bind:value={updateName} class={inputStyle}/>
			<button on:click={handleUpdate} class="bg-green-500/10 p-5 rounded">Update</button>
		</div>
	{/if}
	{#if viewSubPage}
		{#if node?.hasOwnProperty("items")}
			<section class=" p-2 rounded w-[90%] min-w-[400px] transition-all ease-in" use:dndzone={{items:node.items, flipDurationMs, centreDraggedOnCursor: true}}
							 on:consider={handleDndConsider} 
							 on:finalize={handleDndFinalize}>		
					{#each node.items as item(item.id)}
						<div animate:flip="{{duration: flipDurationMs}}" class="p-2 m-3">
							<svelte:self bind:nodes={nodes} node={nodes[item.id]} parentId={node.id} />
							<!-- <svelte:self bind:nodes={nodes} node={nodes[item.id]} nodeChild={true} /> -->
						</div>
					{/each}
							
			</section>
		{/if}
	{/if}

</div>


