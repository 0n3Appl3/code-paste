<script setup>
import { ref, onMounted } from 'vue'
import VanillaTilt from 'vanilla-tilt'
import hljs from 'highlight.js/lib/common'
import 'highlight.js/styles/base16/solar-flare.css'

// Editor Container Styling
const container = ref('editor__container')
const codeContainer = ref(null)
const borderRadius = ref('0.5rem')

// Toolbar Styling
const toolbar = ref('editor__toolbar')
const toolbarClose = ref('editor__toolbar-close')
const toolbarMinimise = ref('editor__toolbar-minimise')
const toolbarMaximise = ref('editor__toolbar-maximise')
const toolbarTitle = ref('editor__toolbar-title')
const toolbarTitleText = ref('Untitled-1')

// Editor Text Field Styling
const textField = ref('editor__text-field')
const textInputText = ref(`class Main {` +
                        `\n   public static void main(String[] args) {` +
                        `\n      System.out.println('Hello World!');` +
                        `\n   }` +
                        `\n}`)
const textInput = ref(null)

// Watermark Styling
const watermark = ref('editor__watermark')
const watermarkText = ref('jeddlupoy.com')

onMounted(() => {
    VanillaTilt.init(codeContainer.value, {
        max: 7,
        scale: 1.07,
        speed: 400,
        perspective: 500,
    })
    resize()
})

// Methods
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
    <div :class="container" ref="codeContainer">
        <div :class="toolbar">
            <div :class="toolbarClose"></div>
            <div :class="toolbarMinimise"></div>
            <div :class="toolbarMaximise"></div>
            <div :class="toolbarTitle"
                 contenteditable="true"
                 spellcheck="false">{{ toolbarTitleText }}</div>
        </div>
        <div v-on:keydown.enter="enter"
             @input="resize" ref="textInput" 
             contenteditable="true" 
             spellcheck="false"
             :class="textField">{{ textInputText }}</div>
        <div :class="watermark">{{ watermarkText }}</div>
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
    overflow: visible;
}
.editor__toolbar > div:last-child {
    margin: auto;
}
.editor__toolbar-close, .editor__toolbar-minimise, .editor__toolbar-maximise {
    display: inline-block;
    width: 0.9rem;
    height: 0.9rem;
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
.editor__toolbar-title {
    display: flex;
    justify-content: center;
    text-align: center;
    font-size: 0.8rem;
    font-weight: bold;
    color: #ddd;
    padding: 0 1rem;
    border-radius: 0.3rem;
    transition: all 0.5s ease-in-out;
}
.editor__toolbar-title:focus, .editor__toolbar-title:hover {
    outline: 1px solid;
}
.editor__toolbar-title, .editor__text-field {
    outline: none;
    border: none;
}
.editor__text-field {
    display: block;
    font-family: Courier, monospace;
    resize: none;
    padding: 0.8rem;
    color: #eee;
    white-space: pre;
    overflow: hidden;
}
.editor__watermark, .editor__text-field {
    background-color: #1b3038;
}
.editor__watermark {
    color: #6d7d85;
    font-size: 0.8rem;
    padding: 0.5rem 1rem;
}
</style>