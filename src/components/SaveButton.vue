<script setup lang="ts">
import { ref } from 'vue'
import { toPng } from 'html-to-image'

const downloadText = ref('Download')

const downloadImage = () => {
    const main = document.getElementsByTagName('main')
    const editor = main[0].lastElementChild

    if (!editor) {
        return;
    }
    toPng(editor as HTMLElement, { 
        pixelRatio: 2,
        style: {
            boxShadow: 'none',
        }
    })
    .then(function (dataUrl) {
        const link = document.createElement('a')
        link.download = 'code.png'
        link.href = dataUrl
        link.click()
        link.remove()
    })
    .catch((error) => {
        console.log(error)
    })
}
</script>

<template>
    <button @click="downloadImage">{{ downloadText }}</button>
</template>

<style scoped>
button {
    padding: 0.7rem 1.7rem;
    margin-top: 2rem;
    text-decoration: none;
    background-color: var(--color-text);
    border: 1px solid var(--color-text);
    border-radius: 0.3rem;
    font-family: Inter, sans-serif;
    transition: all 0.15s ease-in-out;
}
button:hover {
    cursor: pointer;
    color: var(--color-text);
    background-color: var(--color-background-mute);
    transform: scale(1.05);
}
button:active {
    transform: scale(0.8);
}
</style>