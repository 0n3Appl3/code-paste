<script setup>
import { ref } from 'vue'

const hello = ref('Hello world!')

// Editor Container Styling
const container = ref('editor__container')
const borderRadius = ref('0.5rem')

// Toolbar Styling
const toolbar = ref('editor__toolbar')
const toolbarClose = ref('editor__toolbar-close')
const toolbarMinimise = ref('editor__toolbar-minimise')
const toolbarMaximise = ref('editor__toolbar-maximise')

// Editor Text Field Styling
const textField = ref('editor__text-field')
</script>

<template>
    <div :class="container">
        <div :class="toolbar">
            <div :class="toolbarClose"></div>
            <div :class="toolbarMinimise"></div>
            <div :class="toolbarMaximise"></div>
        </div>
        <textarea name="code" :class="textField" placeholder="console.log('Hello World!');"></textarea>
    </div>
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
    border-radius: v-bind(borderRadius) v-bind(borderRadius) 0 0;
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
.editor__text-field {
    background-color: #1b3038;
    outline: none;
    border: none;
    border-radius: 0 0 v-bind(borderRadius) v-bind(borderRadius);
    padding: 0.8rem;
    color: #eee;
}
.editor__text-field::placeholder {
    color: #eee;
}
</style>