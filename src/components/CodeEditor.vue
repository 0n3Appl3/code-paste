<script setup>
import { ref } from 'vue'
import hljs from 'highlight.js'

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
const textInput = ref(null)

const resize = () => {
    const element = textInput.value
    const caretPosition = getCaretCharacterOffsetWithin()
    const div = document.createElement('div')
    div.innerHTML = hljs.highlightAuto(element.innerText).value
    element.textContent = null
    element.appendChild(div)
    setCaretPosition(element, caretPosition)
    element.style.height = 'auto'
    element.style.height = element.scrollHeight + 'px'
    element.style.width = 'auto'
    element.style.width = (element.scrollWidth + 15) + 'px'
}

const enter = () => {
    setTimeout(function() {
        setCaretPosition(textInput.value, getCaretCharacterOffsetWithin() + 1)
    }, 1)
}

// Solution credit: https://stackoverflow.com/questions/36869503/set-caret-position-in-contenteditable-div-that-has-children
const setCaretPosition = (element, position) => {
    for (let node of element.childNodes) {
        if (node.nodeType == Node.TEXT_NODE) {
            if (node.length >= position) {
                const range = document.createRange()
                const selection = window.getSelection()
                range.setStart(node, position)
                range.collapse(true)
                selection.removeAllRanges()
                selection.addRange(range)
                return -1
            } else {
                position -= node.length
            }
        } else {
            position = setCaretPosition(node, position)
            if (position == -1) {
                return -1
            }
        }
    }
    return position;
}

// Solution credit: http://jsfiddle.net/TjXEG/900/
const getCaretCharacterOffsetWithin = () => {
    const element = textInput.value
    let caretOffset = 0
    const doc = element.ownerDocument || element.document
    const win = doc.defaultView || doc.parentWindow
    let sel = ''
    if (typeof win.getSelection != "undefined") {
        sel = win.getSelection()
        if (sel.rangeCount > 0) {
            const range = win.getSelection().getRangeAt(0)
            const preCaretRange = range.cloneRange()
            preCaretRange.selectNodeContents(element)
            preCaretRange.setEnd(range.endContainer, range.endOffset)
            caretOffset = preCaretRange.toString().length
        }
    } else if ( (sel = doc.selection) && sel.type != "Control") {
        const textRange = sel.createRange()
        const preCaretTextRange = doc.body.createTextRange()
        preCaretTextRange.moveToElementText(element)
        preCaretTextRange.setEndPoint("EndToEnd", textRange)
        caretOffset = preCaretTextRange.text.length
    }
    return caretOffset
}
</script>

<template>
    <div :class="container">
        <div :class="toolbar">
            <div :class="toolbarClose"></div>
            <div :class="toolbarMinimise"></div>
            <div :class="toolbarMaximise"></div>
        </div>
        <!-- <textarea @input="resize" ref="textInput" name="code" :class="textField" placeholder="Enter your code"></textarea> -->
        <div v-on:keydown.enter="enter"
             @input="resize" ref="textInput" 
             contenteditable="true" 
             :class="textField"></div>
    </div>
</template>

<style scoped>
.editor__container {
    display: block;
    width: 100%;
    box-shadow: 0 22px 70px 4px rgba(0, 0, 0, 0.56); 
    border-radius: v-bind(borderRadius);
    overflow: hidden;
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
.editor__text-field {
    display: block;
    background-color: #1b3038;
    font-family: 'Courier New', Courier, monospace;
    outline: none;
    resize: none;
    border: none;
    padding: 0.8rem;
    color: #eee;
    white-space: pre;
    overflow: hidden;
}
.editor__text-field::placeholder {
    color: #eee;
}
</style>