<script setup lang="ts">
import CodeEditor from './components/CodeEditor.vue'
</script>

<template>
	<header>
		<h1>Code Paste</h1>
		<p>Paste. Edit. Share. By <a href="https://jeddlupoy.com/" target="_blank">Jedd Lupoy</a>.</p>
	</header>
	<main>
		<CodeEditor />
	</main>
</template>

<style scoped>
header {
	display: block;
	line-height: 1.5;
	text-align: center;
	margin: auto;
}

header > h1 {
	font-weight: bold;
}

main {
	padding-top: 5rem;
}
</style>
