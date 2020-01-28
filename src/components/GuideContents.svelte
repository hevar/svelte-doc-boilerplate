<script>
	import { afterUpdate } from 'svelte';
	import Icon from './Icon.svelte';

	export let sections = [];
	export let active_section = null;
	export let show_contents;
	export let prevent_sidebar_scroll = false;

	let ul;
	let acta = null;

	let shouldShow = false;
	let parent = "";


	afterUpdate(() => {
		// bit of a hack — prevent sidebar scrolling if
		// TOC is open on mobile, or scroll came from within sidebar
		if (prevent_sidebar_scroll || show_contents && window.innerWidth < 832) return;

		const active = ul.querySelector('.active');

		console.log(sections);
		console.log(active_section);
		console.log(".....",acta);

		for (let i = 0; i < sections.length; i++) {
			const arr = sections[i].subsections;

			const arr2 = [];

			arr.forEach(element => arr2.push(element.slug));

		}



		//for (let i = 0; i < sections.length; i++) {
		//	console.log("a:",sections[i].slug);
		//	if (sections[i].slug === active_section) {
		//		acta = sections[i].slug;
		//	}
		//	for (let j = 0; sections[i].subsections.length; j++) {
		//		console.log("--a--",sections[i].subsections[j]);

		//	}
		//}





		if (active) {
			const { top, bottom } = active.getBoundingClientRect();

			const min = 200;
			const max = window.innerHeight - 200;

			if (top > max) {
				ul.parentNode.scrollBy({
					top: top - max,
					left: 0,
					behavior: 'smooth'
				});
			} else if (bottom < min) {
				ul.parentNode.scrollBy({
					top: bottom - min,
					left: 0,
					behavior: 'smooth'
				});
			}
		}
	});
</script>

<style>
	.reference-toc li {
		display: block;
		line-height: 1.2;
		margin: 0 0 4rem 0;
	}

	a {
		position: relative;
		transition: color 0.2s;
		border-bottom: none;
		padding: 0;
		color: var(--second);
	}

	.section {
		display: block;
		padding: 0 0 .8rem 0;
		font-size: var(--h6);
		text-transform: uppercase;
		letter-spacing: 0.1em;
		font-weight: 600;
	}

	.subsection {
		display: block;
		font-size: 1.6rem;
		font-family: var(--font);
		padding: 0 0 0.6em 0;
	}

	.section:hover,
	.subsection:hover,
	.active {
		color: var(--flash);
	}

	.subsection[data-level="4"] {
		padding-left: 1.2rem;
	}

	.icon-container {
		position: absolute;
		top: -.2rem;
		right: 2.4rem;
	}

	@media (min-width: 832px) {
		a {
			color: var(--sidebar-text);
		}

		a:hover,
		.section:hover,
		.subsection:hover,
		.active {
			color: white
		}
	}
</style>

<ul
	bind:this={ul}
	class="reference-toc"
	on:mouseenter="{() => prevent_sidebar_scroll = true}"
	on:mouseleave="{() => prevent_sidebar_scroll = false}"
>
	{#each sections as section}
		<li>
			<a class="section" class:active="{section.slug === active_section}" href="docs#{section.slug}">
				{@html section.metadata.title}

				{#if section.slug === active_section}
					<div class="icon-container">
						<Icon name="arrow-right" />
					</div>
				{/if}
			</a>

			{#if shouldShow}

				{#each section.subsections as subsection}
				<!-- see <script> below: on:click='scrollTo(event, subsection.slug)' -->

					<a
						class="subsection"
						class:active="{subsection.slug === active_section}"
						href="docs#{subsection.slug}"
						data-level="{subsection.level}"
					>
						{@html subsection.title}

						{#if subsection.slug === active_section}
							{parent = subsection.parent}
							<div class="icon-container">
								<Icon name="arrow-right" />
							</div>
						{/if}
					</a>
				{/each}
			{/if}



		</li>
	{/each}
</ul>
