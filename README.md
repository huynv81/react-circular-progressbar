# React Circular Progressbar

[![npm version](https://badge.fury.io/js/react-circular-progressbar.svg)](https://www.npmjs.com/package/react-circular-progressbar)
[![Build Status](https://travis-ci.org/kevinsqi/react-circular-progressbar.svg?branch=master)](https://travis-ci.org/kevinsqi/react-circular-progressbar)
[![Bundle size](https://img.shields.io/bundlephobia/min/react-circular-progressbar.svg)](https://bundlephobia.com/result?p=react-circular-progressbar)

A circular progressbar component, built with SVG and extensively customizable. [**Try it out on CodeSandbox**](https://codesandbox.io/s/vymm4oln6y).

<a href="https://codesandbox.io/s/vymm4oln6y"><img height="120" src="/demo/public/images/animated-progressbar.gif?raw=true" alt="animated progressbar" /></a> <a href="https://codesandbox.io/s/vymm4oln6y"><img height="120" src="/demo/public/images/circular-progressbar-examples.png?raw=true" alt="progressbar examples" /></a>

## Installation

Install with yarn:

```bash
yarn add react-circular-progressbar
```

or npm:

```bash
npm install --save react-circular-progressbar
```

## Usage

Import the component and default styles:

```javascript
import { CircularProgressbar } from 'react-circular-progressbar';
import 'react-circular-progressbar/dist/styles.css';
```

**Note**: Importing CSS requires a CSS loader (if you're using create-react-app, this is already set up for you). If you don't have a CSS loader, you can copy [styles.css](src/styles.css) into your project instead.

Now you can use the component:

```jsx
const percentage = 66;

<CircularProgressbar value={percentage} text={`${percentage}%`} />;
```

If your values are not in percentages, you can adjust `minValue` and `maxValue` to select the scale you want:

```jsx
const value = 0.66;

<CircularProgressbar value={value} maxValue={1} text={`${value * 100}%`} />;
```

## Props

[**Take a look at the CodeSandbox**](https://codesandbox.io/s/vymm4oln6y) for interactive examples on how to use these props.

| Name                | Description                                                                                                                                                                                                                            |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `value`             | Completion value of the progressbar, from `minValue` to `maxValue`. Required.                                                                                                                                                          |
| `minValue`          | Minimum value of the progressbar. Default: `0`.                                                                                                                                                                                        |
| `maxValue`          | Maximum value of the progressbar. Default: `0`.                                                                                                                                                                                        |
| `className`         | Classes to apply to the svg element. Default: `''`.                                                                                                                                                                                    |
| `text`              | Text to display inside progressbar. Default: `null`.                                                                                                                                                                                   |
| `strokeWidth`       | Width of circular line relative to total width of component, a value from 0-100. Default: `8`.                                                                                                                                         |
| `background`        | Whether to display background color. Default: `false`.                                                                                                                                                                                 |
| `backgroundPadding` | Padding between background and edge of svg as a percentage relative to total width of component. Default: `null`.                                                                                                                      |
| `counterClockwise`  | Whether to rotate progressbar in counterclockwise direction. Default: `false`.                                                                                                                                                         |
| `circleRatio`       | Number from 0-1 representing ratio of the full circle diameter the progressbar should use. Default: `1`.                                                                                                                               |
| `classes`           | Object allowing overrides of classNames of each svg subcomponent (root, trail, path, text, background). Enables styling with react-jss. See [this PR](https://github.com/kevinsqi/react-circular-progressbar/pull/25) for more detail. |
| `styles`            | Object allowing customization of styles of each svg subcomponent (root, trail, path, text, background).                                                                                                                                |

Version 1.0.0 removed the `classForPercentage` and `textForPercentage` props in favor of the newer `className` and `text` props. Take a look at the [migration guide](/CHANGELOG.md) for instructions on how to migrate.

## Theming (customizing styles)

Use CSS or inline styles to customize the styling - the default CSS is a good starting point, but you can override it as needed.

#### Using the `styles` prop

You can use the `styles` prop to customize each part of the progressbar (the root svg, path, trail, text, and background). This uses the native `style` prop for each subcomponent, so you can use any CSS properties here, not just the ones mentioned below.

As a convenience, you can use `buildStyles` to configure the most common style changes:

```jsx
import { CircularProgressbar, buildStyles } from 'react-circular-progressbar';

const percentage = 66;

<CircularProgressbar
  value={percentage}
  text={`${percentage}%`}
  styles={buildStyles({
    // Rotation of path and trail, in number of turns (0-1)
    rotation: 0.25,

    // Whether to use rounded or flat corners on the ends - can use 'butt' or 'round'
    strokeLinecap: 'butt',

    // Text size
    textSize: '16px',

    // How long animation takes to go from one percentage to another, in seconds
    pathTransitionDuration: 0.5,

    // Can specify path transition in more detail, or remove it entirely
    // pathTransition: 'none',

    // Colors
    pathColor: `rgba(62, 152, 199, ${percentage / 100})`,
    textColor: '#f88',
    trailColor: '#d6d6d6',
    backgroundColor: '#3e98c7',
  })}
/>;
```

`buildStyles` is a shorthand, but you can also build the `styles` object yourself. It's an object with `root`, `path`, `trail`, `text`, and `background` properties, which are each a set of inline styles to apply to the relevant SVG subcomponent. Here's the equivalent set of styles as above, without using `buildStyles`:

```jsx
<CircularProgressbar
  value={percentage}
  text={`${percentage}%`}
  styles={{
    // Customize the root svg element
    root: {},
    // Customize the path, i.e. the "completed progress"
    path: {
      // Path color
      stroke: `rgba(62, 152, 199, ${percentage / 100})`,
      // Whether to use rounded or flat corners on the ends - can use 'butt' or 'round'
      strokeLinecap: 'butt',
      // Customize transition animation
      transition: 'stroke-dashoffset 0.5s ease 0s',
      // Rotate the path
      transform: 'rotate(0.25turn)',
      transformOrigin: 'center center',
    },
    // Customize the circle behind the path, i.e. the "total progress"
    trail: {
      // Trail color
      stroke: '#d6d6d6',
      // Whether to use rounded or flat corners on the ends - can use 'butt' or 'round'
      strokeLinecap: 'butt',
      // Rotate the trail
      transform: 'rotate(0.25turn)',
      transformOrigin: 'center center',
    },
    // Customize the text
    text: {
      // Text color
      fill: '#f88',
      // Text size
      fontSize: '16px',
    },
    // Customize background - only used when the `background` prop is true
    background: {
      fill: '#3e98c7',
    },
  }}
/>
```

However, you're not limited to the CSS properties shown above&mdash;you have the full set of SVG CSS properties available to you when you use `prop.styles`.

See the [CodeSandbox examples](https://codesandbox.io/s/vymm4oln6y) for a live example on how to customize styles.

#### Using CSS

You can also customize styles with CSS. There are equivalent CSS hooks for the root, path, trail, text, and background of the progressbar.

If you're importing the default styles, you can override the defaults like this:

```jsx
import 'react-circular-progressbar/dist/styles.css';
import './custom.css';
```

```css
// custom.css
.CircularProgressbar-path {
  stroke: red;
}
.CircularProgressbar-trail {
  stroke: gray;
}
.CircularProgressbar-text {
  fill: yellow;
}
.CircularProgressbar-background {
  fill: green;
}
```

## Adding arbitrary text or content inside the progressbar

If you want to add multiple lines of text or images within the progressbar, you can overlay it on top of a regular `<CircularProgressbar />` using absolute positioning. `react-circular-progressbar` ships with a `CircularProgressbarWithChildren` component which makes it easy to do that by using JSX children:

```jsx
import { CircularProgressbarWithChildren } from 'react-circular-progressbar';

<CircularProgressbarWithChildren value={66}>
  {/* Put any JSX content in here that you'd like. It'll be vertically and horizonally centered. */}
  <img style={{ width: 40, marginTop: -5 }} src="https://i.imgur.com/b9NyUGm.png" alt="doge" />
  <div style={{ fontSize: 12, marginTop: -5 }}>
    <strong>66%</strong> mate
  </div>
</CircularProgressbarWithChildren>;
```

<img src="/demo/public/images/CircularProgressbarWithChildren.png?raw=true" alt="CircularProgressbarWithChildren example" />

`CircularProgressbarWithChildren` has all the same props as `CircularProgressbar` - you can use it the exact same way otherwise.

## Animating text

If you want to animate the text as well as the path, you'll need to transition the `value` prop from one value to another using a third-party animation library like `react-move` and an easing library like `d3-ease`.

You can use a render prop wrapper like **[AnimatedProgressProvider.tsx](demo/src/AnimatedProgressProvider.tsx)** to help manage the transitioning value, and use it like this:

```jsx
import { easeQuadInOut } from 'd3-ease';

<AnimatedProgressProvider
  percentageStart={0}
  percentageEnd={66}
  duration={1.4}
  easingFunction={easeQuadInOut}
>
  {(percentage) => {
    const roundedPercentage = Math.round(percentage);
    return (
      <CircularProgressbar
        value={percentage}
        text={`${roundedPercentage}%`}
        {/* This is important to include, because if you're fully managing the
        animation yourself, you'll want to disable the CSS animation. */}
        styles={buildStyles({ pathTransition: 'none' })}
      />
    );
  }}
</AnimatedProgressProvider>;
```

## Fixing text centering in Internet Explorer (IE)

Because the `dominant-baseline` CSS property does not work in IE, the text may not be centered in IE.

A solid cross-browser way to fix this is to use [this approach for overlaying arbitrary content inside the progressbar](https://github.com/kevinsqi/react-circular-progressbar#customizing-the-textcontent-inside-progressbar).

However, if you don't want to do that, you can also work around this by setting the `text` prop to be a `<tspan>` element and then adjusting the `dy` vertical offset, like so:

```jsx
// Use feature or browser detection to determine if IE
const needDominantBaselineFix = ...

<CircularProgressbar
  value={percentage}
  text={<tspan dy={needDominantBaselineFix ? -10 : 0}>{percentage}</tspan>}
/>
```

[See this Codesandbox example](https://codesandbox.io/s/x8o1zx7j4) to see this in action.

## Advanced usage

- [Delaying the animation until the progressbar is visible](https://github.com/kevinsqi/react-circular-progressbar/issues/64)
- [Using a different value range than 0-100](https://codesandbox.io/s/6z64omwv3n)
- [Rotating the progressbar by some degree](https://github.com/kevinsqi/react-circular-progressbar/issues/38)
- [Applying a gradient to the progressbar](https://github.com/kevinsqi/react-circular-progressbar/issues/31#issuecomment-338216925)
- [Customizing the background](https://github.com/kevinsqi/react-circular-progressbar/issues/21#issuecomment-336613160)
- [Creating a countdown timer](https://github.com/kevinsqi/react-circular-progressbar/issues/52)
- [Creating a dashboard/speedometer style progressbar](https://github.com/kevinsqi/react-circular-progressbar/issues/49)

## Supported platforms

react-circular-progressbar does not work with React Native, because React Native does not support `<svg>` out of the box.

## Contributing

Take a look at [CONTRIBUTING.md](/CONTRIBUTING.md) to see how to help contribute to react-circular-progressbar.

## License

[MIT](/LICENSE)
