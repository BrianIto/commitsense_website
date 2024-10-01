<script lang="ts">
	import { onMount } from 'svelte';
	import * as d3 from 'd3';

	let d3container: HTMLDivElement | undefined = undefined;

	onMount(() => {
		let svgElement = d3
			.select(d3container!)
			.append('svg')
			.style('width', '100%')
			.style('height', '100vh');

		let centerX = svgElement.node()!.clientWidth / 2;
		let centerY = svgElement.node()!.clientHeight / 2;

		let commitSpacing = 177;

		const branchOut = (index: number) => {
			svgElement
				.append('path')
				.attr('width', '33')
				.attr('height', '84')
				.attr('d', 'M3 83V51.6957C3 36.7888 3 3 33 3')
				.attr('stroke', '#FFA6A6')
				.attr('stroke-width', '6')
				.attr('transform', `translate(${commitSpacing * index + 72 - 3}, ${centerY - 84})`)
				.attr('fill', 'transparent');
		};

		const lineBranch = (index: number) => {
			svgElement
				.append('path')
				.attr('d', `M30 3H${commitSpacing - 30}`)
				.attr('stroke', '#FFA6A6')
				.attr('stroke-width', '6')
				.attr('transform', `translate(${commitSpacing * index + 72}, ${centerY - 84})`)
				.attr('fill', 'transparent');
		};

		const branchMerge = (index: number) => {
			svgElement
				.append('path')
				.attr('width', '33')
				.attr('height', '84')
				.attr('d', 'M3 83V51.6957C3 36.7888 3 3 33 3')
				.attr('stroke', '#FFA6A6')
				.attr('stroke-width', '6')
				.attr(
					'transform',
					`translate(${commitSpacing * (index + 1) + 3 + 72}, ${centerY - 84}) scale(-1,1)`
				)
				.attr('fill', 'transparent');
		};

		const commitCircle = (index: number) => {
			svgElement
				.append('circle')
				.attr('cx', commitSpacing * index + 72)
				.attr('cy', centerY)
				.attr('r', 14)
				.attr('fill', '#FFA6A6');
		};

		const createBranch = (index: number) => {
			branchOut(index);
			lineBranch(index);
			branchMerge(index);
			commitCircle(index);
			commitCircle(index + 1);
		};

		svgElement
			.append('line')
			.attr('x1', 0)
			.attr('y1', centerY)
			.attr('x2', centerX * 2)
			.attr('y2', centerY)
			.attr('stroke', 'gray')
			.attr('stroke-width', '6px');

		createBranch(0);
		createBranch(1);
		createBranch(2);


	});
</script>

<div bind:this={d3container}></div>

<style>
	:global(body) {
		background: linear-gradient(125deg, #201f1f 30.5%, #212947 123.73%);
	}
</style>
