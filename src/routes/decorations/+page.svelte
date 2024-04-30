<script lang="ts">
	import { codeToHtml } from 'shiki';

	let code = `let banana = 🍌`;
	let html = '';

	(async () => {
		html = await codeToHtml(code, {
			lang: 'ts',
			theme: 'poimandres',
			decorations: [
				{
					start: 4,
					end: 10,
					properties: { class: 'banana' }
				}
			]
		});
	})();
</script>

{@html html}

<style>
	:global(.banana) {
		border: 1px solid oklch(94% 0.2 100);
		padding: 2px;
		border-radius: 2px;
	}
</style>
