<script>
    import DndPageSubPage from '../lib/components/DNDPageSubPage.svelte';
	let nodes = {
		node1:{
				name:'Home',
				id:'home',
				items:[ 
					{id: 'courses'},
					{id: 'experience'},
					{id: 'services'}
				 ], 
		},
		courses: {
			name:'courses', id:"courses"
		},
		experience: {
			name:'experience', id:"experience",items:[]
		},
		services: {
			name:'services',
			id:"services",
			 items:[
				 {id: 'service1'},
				 {id: 'service2'},
				 {id: 'service3'},
				 {id: 'service4'}
			 ],
		},
		service1: {
			name:'service1', id:"service1"
		},
		service2: {
			name:'service2', id:"service2"
		},
		service3: {
			name:'service3', id:"service3"
		},
		service4: {
			name:'service4', id:"service4"
		},
	
	}
	
	console.log("Running")
	
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
		let  pageName = newPageName
		let  pageURl = newPageName.toLowerCase().replace(/ /g, '-')
		let pageObj = {
			name : pageName, id: pageURl , items:[]
		}
		console.log(pageObj)
		newPageName = ""
		//Add Node to Home items 
		nodes.node1.items.push({id:pageURl})

		//Add New Page to Main Obj
		nodes = {...nodes, [pageURl] : pageObj}
		console.log(nodes)
		
	}
	let inputStyle="p-5 bg-white/10 border-slate-500 focus:border-none "
</script>
<div class="flex flex-col items-center justify-center gap-10 py-10 px-3">
	<h3 class="p-2 font-bold text-5xl text-transparent capitalize bg-clip-text bg-gradient-to-r from-purple-400 to-pink-600">
		Try drag-n-drop
	</h3>
	
	<div class="p-5">
		<input bind:value={newPageName} class={inputStyle} placeholder="Type Page Name Here"/>
		<button class="bg-green-500/10 p-5 rounded" on:click={handleAddNewPage}>Add New Page</button>
	</div>
	
	<div class="w-full md:max-w-[80%] lg:max-w-[60%]">
		<DndPageSubPage inputStyle={inputStyle} node={nodes.node1} bind:nodes={nodes} parentId={parentId}/>
	</div>

</div>
<footer class=" text-center italic text-sm text-transparent capitalize bg-clip-text bg-gradient-to-r from-cyan-500 to-pink-300">
	Design & Updated by Kaies Mahmud Nehal
</footer>




