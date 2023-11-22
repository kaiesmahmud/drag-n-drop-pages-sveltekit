<script>
    import { writable } from "svelte/store";
    import Navbar from "../lib/components/Navbar.svelte";
	import ParentDnd from "../lib/components/ParentDND.svelte";

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
	let MenuListResult = writable(nodes)
	let editNavbar = writable(false) ;
	const handleEditNavbar = ()=> {
		console.log("handleEditNavbar Clicked result - ", $editNavbar)
		editNavbar.set(!$editNavbar)
	}
</script>

<svelte:head>
    <title>Drag-N-Drop Page Name Editor Sveltekit</title> 
	<meta name="description" content="Experience effortless page name editing with the Drag-N-Drop Page Name Editor in SvelteKit by Kaies Mahmud Nehal. Seamlessly designed using Tailwind CSS and Svelte-DND-Action, enriched with Iconify icons. Elevate your web development with intuitive drag-and-drop functionality. Unlock user-friendly design and efficiency in one powerful tool" />
</svelte:head>
<div class="w-[100%] min-h-screen">
	<div class=" flex flex-col items-start md:items-center  justify-center gap-10 py-10 px-3">
		<h3 class="p-2 font-bold text-4xl lg:text-8xl text-transparent capitalize bg-clip-text bg-gradient-to-r from-purple-400 to-pink-600">
			Try drag-n-drop
		</h3>
		<h3 class="p-2 font-bold text-4xl lg:text-8xl text-transparent capitalize bg-clip-text bg-gradient-to-r from-purple-400 to-pink-600">
			NavBar Editor
		</h3>
		{#if !$editNavbar}
			<div class="w-[100%]">
				<Navbar nodes={$MenuListResult}/>
			</div>
			<div class="w-[100%] flex items-center justify-center">
				<button class="bg-green-500/20 p-5 rounded  " on:click={handleEditNavbar}>Edit Navbar</button>
			</div>
		{/if}
		{#if $editNavbar}
			<ParentDnd handleEditNavbar={handleEditNavbar} {MenuListResult} nodes={$MenuListResult}/>
		{/if}
	</div>
</div>
<footer class="m-2 text-center italic text-sm text-transparent capitalize bg-clip-text bg-gradient-to-r from-cyan-500 to-pink-300">
	Design & Developed by Kaies Mahmud Nehal | 2023 - November
</footer>
