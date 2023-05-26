<script setup>
import { ref } from 'vue'

const hello = ref('Hello world!')

// Editor Container Styling
const container = ref('editor__container')

// Toolbar Styling
const toolbar = ref('editor__toolbar')
const toolbarClose = ref('editor__toolbar-close')
const toolbarMinimise = ref('editor__toolbar-minimise')
const toolbarMaximise = ref('editor__toolbar-maximise')
</script>

<template>
    <div :class="container">
        <div :class="toolbar">
            <div :class="toolbarClose"></div>
            <div :class="toolbarMinimise"></div>
            <div :class="toolbarMaximise"></div>
        </div>
    </div>
    <textarea name="code" placeholder="console.log('Hello World!');"></textarea>
</template>

<style scoped>
.editor__container {
    display: block;
    width: 100%;
}
.editor__toolbar {
    display: flex;
    align-items: center;
    padding: 0.5rem 0.8rem;
    background-color: #59676e;
}
.editor__toolbar-close, .editor__toolbar-minimise, .editor__toolbar-maximise {
    display: inline-block;
    width: 1rem;
    height: 1rem;
    margin-right: 0.5rem;
    border-radius: 50%;
}
.editor__toolbar-close {
    background-color: #ff605c;
}
.editor__toolbar-minimise {
    background-color: #ffbd44;
}
.editor__toolbar-maximise {
    background-color: #00ca4e;
}
</style>