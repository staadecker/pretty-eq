<script lang="ts">
	import 'mathlive/fonts.css'; // Ensures fonts are imported
	import { MathfieldElement } from 'mathlive';
	import { onMount } from 'svelte';

	let mathFieldDiv: HTMLDivElement;
	let mf: MathfieldElement;
	let popup: HTMLDivElement;
	let mouse_in_popup = false;

	const COLORS = ['black', 'red', 'yellow', 'green', 'blue', 'purple'];

	const showPopup = () => {
		const bounds = mf.getElementInfo(mf.position).bounds;
		popup.style.top = `${bounds.bottom}px`;
		popup.style.left = `${bounds.right}px`;
		popup.style.display = 'flex';
	};

	const hidePopup = (e) => {
		if (!mouse_in_popup) popup.style.display = 'none';
		else e.preventDefault();
	};

	const updateSelectionColor = (color) => {
		mf.applyStyle({ color }, { operation: 'set', range: mf.selection.ranges[0] });
	};

	onMount(async () => {
		mf = new MathfieldElement();
        mf.style.fontSize = "1.44em"
		mf.value = 'f(x) = \\sin(x+\\pi)';
		mathFieldDiv.appendChild(mf);

		mf.addEventListener('selection-change', (e) => {
			if (mf.selection.ranges[0][0] !== mf.selection.ranges[0][1]) showPopup(mf);
			else hidePopup(e);
		});
		mf.addEventListener('focusout', (e) => hidePopup(e));
	});
</script>

<div class="grid h-screen place-items-center">
	<div class="border" bind:this={mathFieldDiv}></div>

	<!-- A popup with 5 little buttons of different colors to change the text color -->
	<div
		bind:this={popup}
		role="toolbar"
		tabindex="0"
		on:mouseenter={() => {
			mouse_in_popup = true;
		}}
		on:mouseleave={() => {
			mouse_in_popup = false;
		}}
		class="absolute space-x-2 p-2 bg-gray-200/50 hover:bg-gray-200 rounded-md hidden"
	>
		{#each COLORS as color}
			<!-- <button on:click={() => alert(mf.getValue())}>Get value</button> -->
			<button
				class="w-4 h-4 rounded-full bg-{color}-500 hover:scale-110"
				on:click|preventDefault={() => updateSelectionColor(color)}
			>
			</button>
		{/each}
	</div>
</div>
