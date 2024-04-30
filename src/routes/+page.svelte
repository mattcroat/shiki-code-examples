<script lang="ts">
	import { getHighlighter, type Highlighter } from 'shiki';

	let highlighter: Highlighter;
	let code = `let banana = 🍌`;
	let html = '';

	(async () => {
		highlighter = await getHighlighter({
			themes: ['poimandres'],
			langs: ['ts', 'svelte']
		});
		html = highlighter.codeToHtml(code, {
			lang: 'ts',
			theme: 'poimandres'
		});
	})();
</script>

{@html html}
