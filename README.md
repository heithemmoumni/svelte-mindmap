# SvelteMindmap

> SvelteMindmap is a svelte component for mindnode maps inspired by [react-mindmap](https://github.com/learn-anything/react-mindmap).

![MIT License](https://badgen.net/badge/license/MIT/blue "MIT License")
[![view on npm](https://img.shields.io/npm/v/svelte-mindmap.svg?colorB=red)](https://www.npmjs.com/package/svelte-mindmap)
 [![Svelte v3](https://img.shields.io/badge/svelte-v3-blueviolet.svg)](https://svelte.dev)



<p align="center">
  <img alt="svelte-mindmap" src="https://raw.githubusercontent.com/heithemmoumni/svelte-mindmap/master/mindmap.png" />
</p>


## Installation

```bash
npm install --save svelte-mindmap
```

## Usage

### Bundler (Webpack, Rollup)

```js
import Mindmap from 'svelte-mindmap';

```

```html
let map = {
  title: null,
  nodes: [],
  connections: []
}

<Mindmap 
    map={map}
/>
```

## Props

| Prop            | Type    | Default | Description                                            |
|-----------------|:-------:|---------|--------------------------------------------------------|
| **nodes**       | Array   | [ ]      | Array of objects used to render nodes.                |
| **connections** | Array   | [ ]      | Array of objects used to render connections.          |
| **subnodes**    | Array   | [ ]      | Array of objects used to render subnodes.             |
| **editable**    | Boolean | false   | Enable editor mode, which allows to move around nodes. |

### nodes

Array of objects used to render nodes. Below an example of the node structure.

```json
{
  "text": "python",
  "url": "http://www.wikiwand.com/en/Python_(programming_language)",
  "fx": -13.916222252976013,
  "fy": -659.1641376795345,
  "category": "wiki",
  "note": ""
}
```

The possible attributes are:

- **text**: title of the node
- **url**: url contained in the node
- **fx** and **fy**: coordinates (if not present they'll be generated)
- **category**: category used to generate an emoji
- **note**: note that will be visible on hover

### connections

Array of objects used to render connections. Below an example of the connection
structure.

```json
{
  "source": "python",
  "target": "basics",
  "curve": {
    "x": -43.5535,
    "y": 299.545
  }
}
```

The possible attributes are:

- **source**: title of the node where the connection starts
- **target**: title of the node where the connection ends
- **curve.x** and **curve.y**: coordinates of the control point of a quadratic bezier curve
(if not specified the connection will be straight)

### subnodes
Array of objects used to render subnodes. The structure is the same as for nodes
with two additional attributes:

- **parent**: title of the parent node
- **color**: used for the margin color, needs to be a valid CSS color


## Styling
Here's a list of all CSS classes for styling:

- **.mindmap-svg**: main `svg` element containing the map;
- **.mindmap-node**: `foreignObject` element representing a node;
- **.mindmap-node--editable**: `foreignObject` element representing a node in editor mode;
- **.mindmap-subnode-group-text**: `foreignObject` element containing all subnodes of a given node;
- **.mindmap-subnode-text**: `div` element containing a subnode;
- **.mindmap-connection**: `path` element for each connection;
- **.mindmap-emoji**: `img` tag for emoji


Feedback would be much appreciated, questions, suggestions, issues are more than welcome.

---

If you like to support my open-source contributions and feeling generous, feel free to:

<a href="https://www.buymeacoffee.com/Zukzhjx" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png" alt="Buy Me A Coffee" style="height: auto !important;width: auto !important;" ></a>
