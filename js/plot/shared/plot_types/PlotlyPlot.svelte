<script lang="ts">
	//@ts-nocheck
	import Plotly from "plotly.js-dist-min";
	import { afterUpdate } from "svelte";

	export let value;
	export let target;

	$: plot = value?.plot;

	let plot_div;
	let plotly_global_style;

	function load_plotly_css(): void {
		if (!plotly_global_style) {
			plotly_global_style = document.getElementById("plotly.js-style-global");
			const plotly_style_clone = plotly_global_style.cloneNode();
			target.appendChild(plotly_style_clone);
			for (const rule of plotly_global_style.sheet.cssRules) {
				plotly_style_clone.sheet.insertRule(rule.cssText);
			}
		}
	}

	afterUpdate(() => {
		load_plotly_css();
		let plotObj = JSON.parse(plot);
		plotObj.layout.title
			? (plotObj.layout.margin = { autoexpand: true })
			: (plotObj.layout.margin = { l: 0, r: 0, b: 0, t: 0 });
		Plotly.react(plot_div, plotObj);
	});
</script>

<div data-testid={"plotly"} bind:this={plot_div} />
