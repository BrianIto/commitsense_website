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

		let commitSpacing = 100;

		let actualHeight = 75;
		const curveStartY = actualHeight * 0.6;
		const controlPoint1Y = actualHeight * 0.4;
		const controlPoint2Y = actualHeight * 0.05;
		const endPointX = 33; // Fixed width of 30

		const branchOut = (index: number, heightness: number) => {
			const baseHeight = centerY - actualHeight;
			const height = baseHeight - actualHeight * heightness;
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
				.attr('transform', `translate(${commitSpacing * index + 72 - 3}, ${height})`)
				.attr('fill', 'transparent');
		};

		const lineBranch = (index: number, spacing = 1, heightness: number = 0) => {
			const baseHeight = centerY - actualHeight;
			const height = baseHeight - actualHeight * heightness;

			svgElement
				.append('path')
				.attr('d', `M30 3H${commitSpacing * spacing - 30}`)
				.attr('stroke', '#FFA6A6')
				.attr('stroke-width', '6')
				.attr('transform', `translate(${commitSpacing * index + 72}, ${height})`)
				.attr('fill', 'transparent');
		};

		const branchMerge = (index: number, spacing: number = 1, heightness = 0) => {
			const baseHeight = centerY - actualHeight;
			const height = baseHeight - actualHeight * heightness;

			svgElement
				.append('path')
				.attr('width', '33')
				.attr('height', '84')
				.attr('d', `M3 ${actualHeight}V${curveStartY}C3 ${controlPoint1Y} 3 ${controlPoint2Y} ${endPointX} 3`)
				.attr('stroke', '#FFA6A6')
				.attr('stroke-width', '6')
				.attr(
					'transform',
					`translate(${(commitSpacing * index + 72) + commitSpacing * spacing}, ${height}) scale(-1,1)`
				)
				.attr('fill', 'transparent');
		};

		const commitCircle = (index: number, spacing: number = 1, heightness = 0, isStart = true) => {
			const height =  centerY - (heightness * actualHeight)
			console.log(commitSpacing * spacing * index + 72, commitSpacing, spacing, index)
			svgElement
				.append('circle')
				.attr('cx', (commitSpacing * index + (isStart ? 72 : 69)) + (isStart ? 0 : commitSpacing * (spacing -1)))
				.attr('cy', height)
				.attr('r', 14)
				.attr('fill', '#FFA6A6');
		};

		const createBranch = (index: number, spacing: number = 1, heightness = 0) => {
			branchOut(index, heightness);
			lineBranch(index, spacing, heightness);
			branchMerge(index, spacing, heightness);
			commitCircle(index, spacing, heightness);
			commitCircle(index + 1, spacing, heightness, false);
		};

		svgElement
			.append('line')
			.attr('x1', 0)
			.attr('y1', centerY)
			.attr('x2', centerX * 2)
			.attr('y2', centerY)
			.attr('stroke', 'gray')
			.attr('stroke-width', '6px');

		createBranch(0, 3);
		createBranch(1, 1, 1)
		createBranch(3, 1);
	});
</script>

<div bind:this={d3container}></div>

<style>
	:global(body) {
		background: linear-gradient(125deg, #201f1f 30.5%, #212947 123.73%);
	}
</style>
