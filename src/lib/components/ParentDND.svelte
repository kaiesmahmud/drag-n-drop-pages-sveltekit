<script>
    import DndPageSubPage from '$lib/components/DNDPageSubPage.svelte';
	export let MenuListResult,nodes ,handleEditNavbar
	// const addNewNode = ()=>{
	// 	console.log("addNewNode ")
	// 	//First Add  to the Root Node Obj
		
	// 	// nodes = { ...nodes, node5:{ name:"hello", id: "hello"} }; //node without child
	// 	nodes = { ...nodes, node5:{ name:"hello", id: "node5", items:[]} }; //node with child
		
	// 	//Add to node1 items list to show 
		
	// 	nodes['node1'].items.push({id:"node5"}) //EveryNode Have to add in Node1 or Home 
	// 	console.log(nodes)
	// }
	// const addChildNode = ()=> {
	// 	console.log("AddChildNode ");
	// 	//Here Node5 will be dynamic
		
	// 	nodes.node5.items.push({id: "example",})
	// 	nodes = { ...nodes, example:{ name:"ChangeName Here", id: "example"} }; //node without child
	// 	console.log(nodes.node5.items)
		
	// }

	let parentId ,newPageName;

	const handleAddNewPage = ()=> {
		//Create a Node Object 
		if(newPageName?.length > 3){
			console.log("length ",newPageName.length)
			let  pageName = newPageName
			let  pageURl = newPageName?.toLowerCase().replace(/ /g, '-')
			let pageObj = {
				name : pageName, id: pageURl , items:[]
			}
			console.log(pageObj)
			newPageName = ""
			if(!nodes[pageURl]){
				//Add Node to Home items 
				nodes.node1.items.push({id:pageURl})
		
				//Add New Page to Main Obj
				nodes = {...nodes, [pageURl] : pageObj}
				console.log(nodes)
			}else{
				alert("Duplicate page Name !")
			}
		}else{
			alert("Please Insert a new page Name !")

		}
	}
	let inputStyle="p-3 md:p-5 bg-white/10 border-slate-500 focus:border-none w-[70%] rounded"
	const handleSaveList = () =>{
		console.log(nodes)
		
		MenuListResult.update(()=> nodes)
		handleEditNavbar()
	}
</script>


<div class="p-3 md:p-5 flex gap-1">
    <input bind:value={newPageName} class={inputStyle} placeholder="Type Page Name Here"/>
    <button class="bg-green-500/10 p-5 rounded w-[30%] " on:click={handleAddNewPage}>Add Page</button>
</div>

<div class="w-[90%] md:w-[80%] lg:w-[50%] p-1">
	<DndPageSubPage  node={nodes.node1} bind:nodes={nodes} parentId={parentId}/>
</div>
<button class="bg-green-500/20 p-5 rounded w-[30%] " on:click={handleSaveList}>Save List</button>

	
	





