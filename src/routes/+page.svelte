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

		let actualHeight = 84;
		const curveStartY = actualHeight * 0.6;
		const controlPoint1Y = actualHeight * 0.4;
		const controlPoint2Y = actualHeight * 0.05;
		const endPointX = 33; // Fixed width of 30

		const branchOut = (index: number) => {
			svgElement
				.append('path')
				.attr('width', '33')
				.attr('height', '84')
				.attr(
					'd',
					`M3 ${actualHeight} V${curveStartY} C3 ${controlPoint1Y} 3 ${controlPoint2Y} ${endPointX} 3`
				)
				.attr('stroke', '#FFA6A6')
				.attr('stroke-width', '6')
				.attr('transform', `translate(${commitSpacing * index + 72 - 3}, ${centerY - actualHeight})`)
				.attr('fill', 'transparent');
		};

		const lineBranch = (index: number, spacing = 1) => {
			svgElement
				.append('path')
				.attr('d', `M30 3H${commitSpacing * spacing - 30}`)
				.attr('stroke', '#FFA6A6')
				.attr('stroke-width', '6')
				.attr('transform', `translate(${commitSpacing * spacing * index + 72}, ${centerY - actualHeight})`)
				.attr('fill', 'transparent');
		};

		const branchMerge = (index: number, spacing: number = 1) => {
			svgElement
				.append('path')
				.attr('width', '33')
				.attr('height', '84')
				.attr('d', 'M3 83V51.6957C3 36.7888 3 3 33 3')
				.attr('stroke', '#FFA6A6')
				.attr('stroke-width', '6')
				.attr(
					'transform',
					`translate(${commitSpacing * spacing * (index + 1) + 3 + 72}, ${centerY - 84}) scale(-1,1)`
				)
				.attr('fill', 'transparent');
		};

		const commitCircle = (index: number, spacing: number = 1) => {
			svgElement
				.append('circle')
				.attr('cx', commitSpacing * spacing * index + 72)
				.attr('cy', centerY)
				.attr('r', 14)
				.attr('fill', '#FFA6A6');
		};

		const createBranch = (index: number, spacing: number = 1) => {
			branchOut(index);
			lineBranch(index, spacing);
			branchMerge(index, spacing);
			commitCircle(index);
			commitCircle(index + 1, spacing);
		};

		svgElement
			.append('line')
			.attr('x1', 0)
			.attr('y1', centerY)
			.attr('x2', centerX * 2)
			.attr('y2', centerY)
			.attr('stroke', 'gray')
			.attr('stroke-width', '6px');

		createBranch(0, 2);
		createBranch(2, 1);
	});
</script>

<div bind:this={d3container}></div>

<style>
	:global(body) {
		background: linear-gradient(125deg, #201f1f 30.5%, #212947 123.73%);
	}
</style>
