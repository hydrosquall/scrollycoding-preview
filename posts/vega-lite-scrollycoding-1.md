---
title: "Welcome to Vega-Lite"
excerpt: 'A gentle introduction to the Vega Lite Spec, in which you will turn a vertical bar chart into a horizontal lollipop chart'
coverImage: '/assets/blog/dynamic-routing/cover.jpg'
date: '2021-04-10T05:35:07.322Z'
author:
  name: Cameron Yick
  picture: 'https://avatars.githubusercontent.com/u/9020979?v=4'
ogImage:
  url: '/assets/blog/dynamic-routing/cover.jpg'
---

Vega-Lite is a declarative grammar of graphics. It's a powerful language for building simple and complex visualizations by writing JSON definitions, called "specs".

Full-fledged Vega specs may seem daunting at first. However, they are approachable when you build them up gradually. Let's give it a try.

<Hike>

<StepHead focus="3:13">

<!-- // implicitly displays the unnamed one -->
```jsx
import { VegaLite } from 'react-vega'
import { barData } from './VegaData.js'

const spec = {
  width: 360,
  height: 150,
  mark: 'bar', // Try changing this to "point"
  encoding: {
    x: { field: 'name', type: 'ordinal' },
    y: { field: 'age', type: 'quantitative' },
  },
  data: { name: 'table' },
}

export default function App() {
  return (
    <VegaLite spec={spec} data={barData} />
  )
}
```

```jsx VegaData.js
export const barData = {
  table: [
    { name: 'Alex', age: 28, location: 'Pasadena' },
    { name: 'Bettina', age: 55, location: 'Denver' },
    { name: 'Casper', age: 43, location: 'Athens' },
    { name: 'Dina', age: 31, location: 'Pigeon Forge' },
    { name: 'Elan', age: 51, location: 'Brooklyn' },
    { name: 'JJ', age: 20, location: 'Brooklyn' },
  ],
}
```

</StepHead>

### Step 1: Data and Specs

Let's begin with [a minimal `vega-lite spec`](focus://4:13) from the docs.

We'll inline a table of [fixture data](focus://VegaData.js#1:9). Vega accepts an array of objects that contain the same keys. In this case, we have `name`, `age`, and `location`

Every visualization has at least 2 parts, the `data` and the `spec`. For simplicity, we'll use an inlined dataset.

<details>
<summary> <i>How to use remote data (instead of inlined data)</i>

 [Data loading documentation](https://vega.github.io/vega-lite/docs/data.html)

</summary>

```js
const spec = {
  // ... rest is the same
  data: {
    "url": "https://vega.github.io/vega-lite/data/cars.json"
  }
}
```
</details>

<CodeSlot style={{height: 350}}/>

Lastly, we'll connect these two props to our main [React Component](focus://#15:19).

<PreviewSlot zoom={0.8} style={{height: 350}} />

Hooray, you've built your first bar chart with Vega Lite!


<StepHead focus="7:12">

```jsx
import { VegaLite } from 'react-vega'
import { barData } from './VegaData.js'

const spec = {
  width: 360,
  height: 150,
  mark: { "type": "bar", "tooltip": true },
  encoding: {
    x: { field: 'name', type: 'ordinal' },
    y: { field: 'age', type: 'quantitative' },
    color: { field: 'location', type: 'nominal' },
  },
  data: { name: 'table' },
}

export default function App() {
  return (
    <VegaLite spec={spec} data={barData} />
  )
}
```

```jsx VegaData.js
export const barData = {
  table: [
    { name: 'Alex', age: 28, location: 'Pasadena' },
    { name: 'Beth', age: 55, location: 'Denver' },
    { name: 'Carlo', age: 43, location: 'Athens' },
    { name: 'Dina', age: 31, location: 'Pigeon Forge' },
    { name: 'Elan', age: 51, location: 'Brooklyn' },
    { name: 'Frieda', age: 30, location: 'Brooklyn' },
  ],
}
```

</StepHead>

### Step 2: Colors and Tooltips

Next, let's make use of some additional fields in our data by making use of the color and tooltip channels.

[Set `mark.tooltip`](focus://#7) to add a box with all the columns in your data when each bar is hovered over. You can [customize](https://vega.github.io/vega-lite/docs/tooltip.html) this further if you don't want to include every column.

<CodeSlot style={{height: 350}}/>

[Set `encoding.color`](focus://#11) to add color using the `location` column. You could use `name` as well, but it would be redundant with the x axis labels.

<PreviewSlot zoom={0.8} style={{height: 350}} />


<StepHead focus="11">

```jsx
import { VegaLite } from 'react-vega'
import { barData } from './VegaData.js'

const spec = {
  width: 360,
  height: 150,
  mark: { "type": "bar", "tooltip": true },
  encoding: {
    x: { field: 'name', type: 'ordinal' },
    y: { field: 'age', type: 'quantitative' },
    color: { field: 'location', type: 'nominal', scale: { scheme: 'accent' } },
  },
  data: { name: 'table' }
};

export default function App() {
  return (
    <VegaLite spec={spec} data={barData} />
  )
}
```

```jsx VegaData.js
export const barData = {
  table: [
    { name: 'Alex', age: 28, location: 'Pasadena' },
    { name: 'Beth', age: 55, location: 'Denver' },
    { name: 'Carlo', age: 43, location: 'Athens' },
    { name: 'Dina', age: 31, location: 'Pigeon Forge' },
    { name: 'Elan', age: 51, location: 'Brooklyn' },
    { name: 'Frieda', age: 30, location: 'Brooklyn' },
  ],
}
```

</StepHead>

### Step 3.x Color customization

If you want a different color scheme, there are several built-in [defaults](https://vega.github.io/vega-lite/docs/scale.html#scheme) to choose from. We'll use `accent` just so you can see that another choice is possible, but we probably won't keep it.


<CodeSlot style={{height: 350}}/>


<PreviewSlot zoom={0.8} style={{height: 350}} />


<StepHead focus="9:10">

```jsx
import { VegaLite } from 'react-vega'
import { barData } from './VegaData.js'

const spec = {
  width: 360,
  height: 150,
  mark: { "type": "bar", "tooltip": true },
  encoding: {
    y: { field: 'name', type: 'ordinal' },
    x: { field: 'age', type: 'quantitative' },
    color: { field: 'location', type: 'nominal', scale: { scheme: 'accent' } },
  },
  data: { name: 'table' }
};

export default function App() {
  return (
    <VegaLite spec={spec} data={barData} />
  )
}
```

```jsx VegaData.js
export const barData = {
  table: [
    { name: 'Alex', age: 28, location: 'Pasadena' },
    { name: 'Beth', age: 55, location: 'Denver' },
    { name: 'Carlo', age: 43, location: 'Athens' },
    { name: 'Dina', age: 31, location: 'Pigeon Forge' },
    { name: 'Elan', age: 51, location: 'Brooklyn' },
    { name: 'Frieda', age: 30, location: 'Brooklyn' },
  ],
}
```

</StepHead>

### Step 4: Flip-flop

For fun, let's see how easy it is to invert the roles of the X and Y axes. This also means you won't have to turn your head to read the "names" properly.

By flipping the `x` and `y` keys, we can toggle between horizontal and vertical bar charts.

<StepHead focus="7">

```jsx
import { VegaLite } from 'react-vega'
import { barData } from './VegaData.js'

const spec = {
  width: 150,
  height: 150,
  mark: { "type": "point", "tooltip": true },
  encoding: {
    y: { field: 'name', type: 'ordinal' },
    x: { field: 'age', type: 'quantitative' },
    color: { field: 'location', type: 'nominal' },
  },
  data: { name: 'table' }
};

export default function App() {
  return (
    <VegaLite spec={spec} data={barData} />
  )
}
```

```jsx VegaData.js
export const barData = {
  table: [
    { name: 'Alex', age: 28, location: 'Pasadena' },
    { name: 'Beth', age: 55, location: 'Denver' },
    { name: 'Carlo', age: 43, location: 'Athens' },
    { name: 'Dina', age: 31, location: 'Pigeon Forge' },
    { name: 'Elan', age: 51, location: 'Brooklyn' },
    { name: 'Frieda', age: 30, location: 'Brooklyn' },
  ],
}
```

</StepHead>

### Step 5: Making a point

We don't need to use so much ink to compare these points. Let's convert this to a lollipop chart. This graphical form conveys the same information as a bar chart, but with less space. Lollipops have two parts, the stems and the point.

We'll put each part into its own layer.

First, we'll convert our `bar` mark type to `circle`.

<StepHead focus="7:16">

```jsx
import { VegaLite } from 'react-vega'
import { barData } from './VegaData.js'

const spec = {
  width: 150,
  height: 150,
  layer: [
    {
      mark: { "type": "point", "tooltip": true },
      encoding: {
        y: { field: 'name', type: 'ordinal' },
        x: { field: 'age', type: 'quantitative' },
        color: { field: 'location', type: 'nominal' },
      },
    }
  ],
  data: { name: 'table' }
};

export default function App() {
  return (
    <VegaLite spec={spec} data={barData} />
  )
}
```

```jsx VegaData.js
export const barData = {
  table: [
    { name: 'Alex', age: 28, location: 'Pasadena' },
    { name: 'Beth', age: 55, location: 'Denver' },
    { name: 'Carlo', age: 43, location: 'Athens' },
    { name: 'Dina', age: 31, location: 'Pigeon Forge' },
    { name: 'Elan', age: 51, location: 'Brooklyn' },
    { name: 'Frieda', age: 30, location: 'Brooklyn' },
  ],
}
```

</StepHead>

### Step 5a: Adding a point layer

We'll add a `layer` key to our top level specification. Every `layer` gets stacked along the "z-axis", like papers in a stack.

Any properties missing from a spec will be inherited from the top level Vega spec. For now we'll reuse the top level  "data" or "width/height" properties, but you can override them within individual layers if needed.

If you do this step correctly, your visualization won't look different from the previous step. However, we've unlocked a very powerful ability - the ability to compose new compound visualization types by adding any number of new layers on top of this base layer! You'll see the benefits of this approach very shortly.

<StepHead focus="7:16">

```jsx
import { VegaLite } from 'react-vega'
import { barData } from './VegaData.js'

const spec = {
  width: 150,
  height: 150,
  layer: [
    {
      mark: { "type": "bar", "height": {"band": 0.05 } },
      encoding: {
        y: { field: 'name', type: 'ordinal' },
        x: { field: 'age', type: 'quantitative' },
        color: { value: '#777' }
      },
    },
    {
      mark: { "type": "circle", "tooltip": true },
      encoding: {
        y: { field: 'name', type: 'ordinal' },
        x: { field: 'age', type: 'quantitative' },
        color: { field: 'location', type: 'nominal' },
      },
    }
  ],
  data: { name: 'table' }
};

export default function App() {
  return (
    <VegaLite spec={spec} data={barData} />
  )
}
```

```jsx VegaData.js
export const barData = {
  table: [
    { name: 'Alex', age: 28, location: 'Pasadena' },
    { name: 'Beth', age: 55, location: 'Denver' },
    { name: 'Carlo', age: 43, location: 'Athens' },
    { name: 'Dina', age: 31, location: 'Pigeon Forge' },
    { name: 'Elan', age: 51, location: 'Brooklyn' },
    { name: 'Frieda', age: 30, location: 'Brooklyn' },
  ],
}
```

</StepHead>

### Step 5b: Adding a bar layer

To close this example, we'll add a set of bars to serve as our lollipop stems.

Two new stylistic properties we're using:

1. We can give our bars [relative band size](focus://#9)
2. We can set assign our marks to a [constant color](focus://#13).

### Finish (for now)

Much can and should be written about the expressive capabilities of Vega, but not today.

See this [Observable notebook](https://observablehq.com/@hydrosquall/visually-exploring-nyc-events-from-enigma-public-with-vega-) for more examples of what you can build with Vega Lite.

</Hike>


## Thank you

Stay tuned for more Vega-Lite based scrollycoding episodes.

Built using @pomber's `code-hike` framework, and based on my previous [tutorials](https://observablehq.com/collection/@hydrosquall/tutorials) in Observable Notebooks.
