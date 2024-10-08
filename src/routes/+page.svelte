<script lang="ts">
    import "mathlive/fonts.css" // Ensures fonts are imported

    import { onMount } from "svelte";

    let mathField: HTMLDivElement;

    onMount(async () => {
        // We need to do a dynamic import to avoid MathField being defined after it becomes available? Is this because npm run dev is doing SSR?
        const { MathfieldElement } = await import("mathlive");
        const mfe = new MathfieldElement();
        mfe.value = 'f(x) = \\sin(x+\\pi)';
        mathField.appendChild(mfe);
    });
</script>

<h1>PrettyEQ</h1>
<div bind:this={mathField}></div>