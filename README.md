# Reveal.js plugin - d3js

[Reveal.js](https://github.com/hakimel/reveal.js) plugin to embed d3.js visualizations in slides
with support for transitions triggerd by normal slide navigation. d3.js visualizations are embed as
iframe elements.

[Check out the live demo](https://jlegewie.github.io/demo-revealjs-d3js/index.html)


## Installation

1. Copy the file `d3js.js` into the plugin folder of your reveal.js presentation, i.e. `plugin/d3js`.

2. Add the plugins to the dependencies in your presentation

```javascript
Reveal.initialize({
	// ...
	dependencies: [
		// ... 
		{ src: 'https://d3js.org/d3.v4.min.js' },
		{ src: 'plugin/d3js/d3js.js' },
		// ... 
	]
});
```

### Adding d3.js visualizations to slides

To add a d3.js visualizations to your presentation, simply create a container element with the
class `fig-container`, the attribute `data-fig-id` with the figure id, and `data-file` with the
path to the html page with the d3.js visualizations. The container can be a `div` or `span` element
in a slide or the `section` element to add the visualizations to the background.

In the example, `index.html` contains the following code:

```html
<section>
    <div class="fig-container"
        data-fig-id="fig-collision-detection"
        data-file="Collision-Detection.html"></div>
</section>
```

### Adding transitions

To add transitions, simply create two global variables in the html page with the d3.js
visualizations:

1. `_transitions` is an array with functions that for each transition.

2. `_inverse_transitions` is an array with functions that revert each transition when user
navigates back. `_inverse_transitions` can be set to `[]` if this functionality is not required.

The plugin automatically looks at the number of elements in the array `_transitions` and creates
corresponding fragments in the container slide.

In the example, `Collision-Detection.html` adds two transitions:

```javascript
var _transitions = [],
    _inverse_transitions = [];

_transitions.push(
  () => d3.selectAll("circle")
 			.transition().duration(1000)
 			.attr("r", function(d) { return d.radius * runif(0, 2); })
);
_inverse_transitions.push(
  () => d3.selectAll("circle")
  			.transition().duration(1000)
  			.attr("r", function(d) { return d.radius; })
);
_transitions.push(
  () => d3.selectAll("circle")
  			.transition().duration(1500)
  			.style("fill", "#33ff33")
);
_inverse_transitions.push(
  () => d3.selectAll("circle")
  			.transition().duration(1500)
  			.style("fill", function(d, i) { return color(i % 3); })
);
```


## License

MIT licensed

Copyright (C) 2017 Joscha Legewie
