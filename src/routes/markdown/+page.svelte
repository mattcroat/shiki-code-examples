<script lang="ts">
	import { unified } from 'unified';
	import toMDAST from 'remark-parse';
	import toHAST from 'remark-rehype';
	import rehypeShiki from '@shikijs/rehype';
	import toHtml from 'rehype-stringify';
	import { transformerMetaHighlight } from '@shikijs/transformers';
	import type { ShikiTransformer } from 'shiki';

	let markdown = `
\`\`\`svelte {2,6} showLineNumbers
<script>
	let banana = $state('🍌')
<\/script>

<button onclick={() => banana += '🍌'}>
	{banana}
</button>
\`\`\`
  `;
	let html = '';

	function lines(): ShikiTransformer {
		return {
			name: 'lines',
			code(node) {
				const meta = this.options.meta?.__raw;
				if (!meta) return;
				const showLineNumbers = /showLineNumbers/.test(meta);
				if (showLineNumbers) {
					this.addClassToHast(node, 'lines');
				}
			}
		};
	}

	(async () => {
		const file = await unified()
			.use(toMDAST)
			.use(toHAST)
			.use(rehypeShiki, {
				theme: 'poimandres',
				transformers: [transformerMetaHighlight(), lines()]
			})
			.use(toHtml)
			.process(markdown);
		html = String(file);
	})();
</script>

{@html html}
