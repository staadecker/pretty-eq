<script lang="ts">
    import "mathlive/fonts.css" // Ensures fonts are imported
    import { MathfieldElement } from "mathlive";
    import { onMount } from "svelte";

    let mathField: HTMLDivElement;

    onMount(async () => {
        const mfe = new MathfieldElement();
        mfe.value = 'f(x) = \\sin(x+\\pi)';
        mathField.appendChild(mfe);
    });
</script>

<div class="grid h-screen place-items-center">
    <h1>PrettyEQ</h1>


    <div class="border" bind:this={mathField}></div>

</div>
