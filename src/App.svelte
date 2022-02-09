<script lang="ts">
	import Tailwindcss from "./Tailwindcss.svelte"
	import "mathlive"
	import { tick } from "svelte";

	let mathFields: string[] = ["x"]

	async function input(event: any, index: number) {

		if (!event.target.hasFocus()) return

		if (index + 1 == mathFields.length) {
			mathFields = [...mathFields, ""]
		}
		await tick()
		document.getElementById("math-" + (index + 1)).focus()
	}


</script>
<div>
	{#each mathFields as math, i}
		<math-field 
			virtual-keyboard-mode=manual
			smart-mode=true
			smart-fence=true
			smart-superscript=true
			id="math-{i}"
			on:change={(e) => input(e, i)}
		>
			{math}
		</math-field>
	{/each}
</div>
<Tailwindcss />