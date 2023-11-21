<script>
	import { flip } from 'svelte/animate';
	import { dndzone } from 'svelte-dnd-action';
	
	export let nodes 
	export let node,parentId
	
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
<div class="flex">
	<div>
		<b style="color:{node?.color}">{node?.name}</b> 
		<button on:click={logme}>LogMe</button>
	</div>
		{#if node?.id != "home"}
			{#if node?.hasOwnProperty("items")}
					{#if node?.items?.length != 0}
						<button on:click={handleSubPageView}>{(viewSubPage)?"Hide":"View"}</button>
					{/if}
			<button on:click={handleNeedtoAddNewPage} class="add">{(needtoAddNewPage)?"X":"Add"}</button>
			{/if}
		
		<button on:click={handleUpdate} class="update">Update</button>
		<button on:click={deleteNode} class="delete">Delete</button>
		{/if}
</div>

{#if node?.hasOwnProperty("items")}
	{#if needtoAddNewPage}
		<div class="">
			<input bind:value={subPageName} />
			<button on:click={handleAddSubPages}>Add SubPage</button>
		</div>
	{/if}
{/if}
{#if openUpdateSection}
	<div class="">
		<input bind:value={updateName} />
		<button on:click={handleUpdate}>Update</button>
	</div>
{/if}
{#if viewSubPage}
	{#if node?.hasOwnProperty("items")}
		<section use:dndzone={{items:node.items, flipDurationMs, centreDraggedOnCursor: true}}
						 on:consider={handleDndConsider} 
						 on:finalize={handleDndFinalize}>		
				{#each node.items as item(item.id)}
					<div animate:flip="{{duration: flipDurationMs}}" class="item">
						<svelte:self bind:nodes={nodes} node={nodes[item.id]} parentId={node.id} />
						<!-- <svelte:self bind:nodes={nodes} node={nodes[item.id]} nodeChild={true} /> -->
					</div>
				{/each}
						
		</section>
	{/if}
{/if}


<style>
	section {
		width: auto;
		max-width: 600px;
		border: 0px solid black;
		padding: 0.4em;
        /* this will allow the dragged element to scroll the list */
		overflow-y: auto ;
		height: auto;
		background-color: rgba(100, 100, 100, 0.1); 
	}
	div {
		width: 90%;
		padding: 0.3em;
		/* border: 0px solid blue; */
		margin: 0.15em 0;
	}
	.item{
		background-color: rgba(00, 100, 100, 0.1);
		border: 2px solid white
	}
	.flex{
		display:flex ;
		gap: 20px;
		justify-content: space-between;
	}
	.delete{
		background-color: orangered; 
		padding:5px;
		border-radius:10%;
		border: none;
}
	.update{
		background-color: palevioletred; 
		padding:5px;
		border-radius:10%;
		border: none;
}
	.add{
		background:teal; 
		border-radius:10%;
		border: none;
	}
	input{
		padding: 5px;
	}
</style>