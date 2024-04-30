<script lang="ts">
	import { codeToHtml, type ShikiTransformer } from 'shiki';

	let code = `let banana = 🍌`;
	let html = '';

	function banana(): ShikiTransformer {
		return {
			name: 'banana',
			code(node) {
				this.addClassToHast(node, 'language-ts');
			},
			line(node, line) {
				node.properties['data-line'] = line;
				if ([1, 3, 4].includes(line))
					this.addClassToHast(node, 'highlight');
			},
			span(node, line, col) {
				node.properties['data-token'] =
					`token:${line}:${col}`;
			}
		};
	}

	(async () => {
		html = await codeToHtml(code, {
			lang: 'ts',
			theme: 'poimandres',
			transformers: [banana()]
		});
	})();
</script>

{@html html}
