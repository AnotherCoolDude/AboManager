<script>
import Moveable from "svelte-moveable";
import { onMount } from "svelte";

let filter = [
    {title: 'expenses', el: ''},
    {title: 'earnings', el: ''}, 
    {title: 'household', el: ''},
    {title: 'entertainment', el: ''}
];

let frameMap = new Map();
let moveable;
let target;

function newFrame(target) {
    const frame = {translate: [0, 0]};
    frameMap.set(target, frame);
    return frame;
}

function getFrame(target) {
    setTimeout(() => {});
    return frameMap.get(target) || newFrame(target);
}

onMount(() => {
    target = document.querySelector(".target");
})

function onDragStart(detail) {
    console.log(detail);
    console.log(target);
    detail.set(getFrame(target).translate);
}

function onDrag(detail) {
    getFrame(detail.target).translate = detail.beforeTranslate;
    detail.target.style.transform
        = `translate(${detail.beforeTranslate[0]}px, ${detail.beforeTranslate[1]}px)`;
}

function handleMousedown(event) {
    target = event.target;
    setTimeout(() => {
        moveable.dragStart(event);
    })
}

</script>

<style>

</style>


<div class="box">
    {#each filter as f,i}
    <div class="target tag" on:mousedown={(e) => {
        window.setTimeout(() => {});
        handleMousedown(e);
    }}>{f.title}</div>
    {/each}
    <Moveable
    bind:this={moveable}
    draggable={true}
    target={target}
    throttleDrag={0}
    on:drag={({ detail }) => {
        console.log(detail);
        onDrag(detail);
    }}
    on:dragStart={({ detail }) => {
        console.log(detail);
        onDragStart(detail);
    }}
    />
</div>