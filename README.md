_[Demo and API Docs](http://collaborne.github.io/app-experiment-behavior)_

# app-experiment-behavior [![Bower version](https://badge.fury.io/bo/app-experiment-behavior.svg)](http://badge.fury.io/bo/app-experiment-behavior) [![Build Status](https://travis-ci.org/Collaborne/app-experiment-behavior.svg?branch=master)](https://travis-ci.org/Collaborne/app-experiment-behavior)

Polymer 1.x behavior for simple A/B/... experiments

### Usage

**See this [blog post](https://medium.com/collaborne-engineering/a-b-testing-made-easy-with-polymer-7038b22779af) for a detailed tutorial.**

Using this behavior is (fairly) easy:

`bower install app-experiment-behavior`

After that you need to reference the behavior and provide the viewer information for your component:

```html
<dom-module is="experiment-component">
	<template>
		<style>
			:host {
				margin: 2em;
			}
		</style>

		<div>[[_greeting]]</div>
	</template>

	<script>
		// Simple example for how to set the oracle with a custom extraction function.
		const oracle = Polymer.AppExperiment.Oracle.VIEWER_HASH(viewerId => viewerId);

		Polymer({
			is: 'experiment-component',

			properties: {
				experimentViewer: String,

				_greeting: {
					type: String,
					computed: '_computeGreeting(experiment)'
				}
			},

			behaviors: [
				Polymer.AppExperiment.Behavior('experiment-component:greeting', [ 'world', 'universe' ], { oracle })
			],

			_computeGreeting(experiment) {
				const greetings = {
					'world': 'Hello, World!',
					'universe': 'Jo, Universe!'
				}

				return greetings[experiment];
			}
		})
	</script>
</dom-module>
```

In this example the component provides the 'experimentViewer' property directly, but this could be extracted into a separate behavior, allowing you to simplify code in your app even more:

```js
const MyViewerBehavior = {
	properties: {
		experimentViewer: {
			type: Object,
			computed: '_computeExperimentViewer(...)'
		},
	},

	_computeExperimentViewer(...) {
		// Calculate the viewer based on the given inputs
	}
};

function MyExperimentBehavior() {
	return [...Polymer.AppExperimentBehavior.apply(this, arguments), MyViewerBehavior ];
}
```
