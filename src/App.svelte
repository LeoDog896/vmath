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

	$: mathFields.forEach(async (_, i) => {
		await tick()
		let mf = document.getElementById("math-" + i) as any
		mf.setOptions({
			onKeystroke: (__, keystroke, ___) => {
				if (keystroke === '[Backspace]' && mf.value == "" && mathFields.length > 1) {
					mathFields = [...mathFields.slice(0, i), ...mathFields.slice(i + 1)];
					tick().then(() => {
						if (i == 0) {
							document.getElementById("math-0").focus()
						} else {
							document.getElementById("math-" + (i - 1)).focus()
						}
					})

					return false;
				}
				// Keystroke not handled, return true for default handling to proceed.
				return true;
			}
		});
	})

</script>
<div class="w-1/2">
	{#each mathFields as math, i}
		<math-field
		
			virtual-keyboard-mode=manual
			smart-mode=true
			smart-fence=true
			smart-superscript=true
			id="math-{i}"
			on:change={(e) => input(e, i)}
			on:input={(e) => mathFields[i] = e.target.value}
		>
			{math}
		</math-field>
	{/each}
</div>
<Tailwindcss />