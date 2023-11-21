<script>
    import ParentDnd from "../lib/components/ParentDND.svelte";

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
		<ParentDnd/>
	</div>
</div>
<footer class="m-2 text-center italic text-sm text-transparent capitalize bg-clip-text bg-gradient-to-r from-cyan-500 to-pink-300">
	Design & Developed by Kaies Mahmud Nehal | 2023 - November
</footer>