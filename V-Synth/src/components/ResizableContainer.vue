<template>
    <div class="resize-container" ref="interactableContainer" :style="{ transform: transformString }">
        <h2>Resize-able / Draggable Container</h2>
        <h3>{{transformString}}</h3>
    </div>
</template>

<script setup lang="ts">
    import { computed } from '@vue/reactivity';
    import { reactive, onMounted, onBeforeUnmount, ref } from 'vue';
    import interact from 'interactjs';
    const interactableContainer = ref();
    
    const position = reactive({
        x: 0,
        y: 0,
    });

    const transformString = computed(() => {
        const { x, y } = position;
        return `translate3D(${x}px, ${y}px, 0)`;
    });

    function interactSetPosition(coordinates: {pX: number, pY: number}): void {
        const { pX, pY } = coordinates;
        position.x = pX;
        position.y = pY;
    }

    function resetContainerPosition(): void {
        position.x = 0;
        position.y = 0;
    }

    onMounted(() => {
        interact(interactableContainer.value).draggable({
            onmove: event => {
                const x = position.x + event.dx as number;
                const y = position.y + event.dy as number;
                interactSetPosition({ pX: x, pY: y });
            },
            onend: () => {
                resetContainerPosition();
            }
        });
    });

    onBeforeUnmount(() => {
        interact(interactableContainer.value).unset();
    });

    // TODO: implement draggability and resizability features
    // https://interactjs.io/docs/draggable/
    // https://interactjs.io/docs/resizable/
</script>

<style scoped>
    .resize-container {
        background-color: #5c5c8a;
        opacity: 0.75;
        min-height: 20rem;
        min-width: 30rem;
        max-height: 40rem;
        max-width: 60rem;
    
        resize: both;
        overflow: auto;
    }
</style>