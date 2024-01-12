<script setup lang="ts">
import { ref } from 'vue';
let p = defineProps(["name", "class", "width", "height"]) // Props  (name, class, width, height)

let i: any = ref("") // Icon variable
let local:boolean = false // if you're using this component in electron like frameworks you must set this variable to true
async function get() {
    let uri = `/${p.name}.svg`
    if(local){
        const local = window.location.href.replace("index.html", "")
        uri = local + uri
    }
    let a = await fetch(uri) // Fetching svg file
    if (!a.ok) return
    let b = await a.text() // Converting svg file to text
    b = b.toString() // Converting svg file to string
    const parser = new DOMParser(); // Parsing svg file
    const svgDoc = parser.parseFromString(b, "image/svg+xml"); // Parsing svg file
    if (p.width || p.height) { // If width or height is defined
        svgDoc.documentElement.setAttribute("width", p.width)
        svgDoc.documentElement.setAttribute("height", p.height)
    } else { // If width or height is not defined
        svgDoc.documentElement.removeAttribute("width");
        svgDoc.documentElement.removeAttribute("height");
    }
    i.value = new XMLSerializer().serializeToString(svgDoc); // Writing svg file to variable
}
get() // Calling get function
</script>

<template>
    <div id="cubicon" :class="p.class" v-if="i != ''" v-html="i"></div>
</template>

<style scoped>
#cubicon { /* Core css rules */
    display: flex;
    justify-content: center;
    flex-shrink: 0;
}
</style>