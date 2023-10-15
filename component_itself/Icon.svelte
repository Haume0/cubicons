<script lang="ts">
export let name,width,height

let i: any = "" // Icon variable
async function get() {
    let a = await fetch(`/${name}.svg`) // Fetching svg file
    if (!a.ok) return
    let b = await a.text() // Converting svg file to text
    b = b.toString() // Converting svg file to string
    const parser = new DOMParser(); // Parsing svg file
    const svgDoc = parser.parseFromString(b, "image/svg+xml"); // Parsing svg file
    if (width || height) { // If width or height is defined
        svgDoc.documentElement.setAttribute("width", width)
        svgDoc.documentElement.setAttribute("height", height)
    } else { // If width or height is not defined
        svgDoc.documentElement.removeAttribute("width");
        svgDoc.documentElement.removeAttribute("height");
    }
    i = new XMLSerializer().serializeToString(svgDoc); // Writing svg file to variable
}
get() // Calling get function
</script>

{#if "i != ''"}
    <cubicon class={$$props.class}>{@html i}</cubicon>
{/if}

<style scoped>
cubicon { /* Core css rules */
    display: flex;
    justify-content: center;
    flex-shrink: 0;
}
</style>