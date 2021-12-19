---
title: Componentization Tips
date: 2021-12-12 12:00:00
tags:
    - reactjs
    - components
---

Hello everyone, and welcome to my blog.

Today I'll be talking about the tips (rules?) I keep in mind when makng components in ReactJS.

# Tip 1: The Scroll Rule

One of the  more popular rules that's used in ReactJS is the scroll rule. This states that a component shouldn't be so long that you need to scroll to see the entirety of it. For example, take the following piece of code:

```javascript
import React from "react";

const ExampleComponent = () => {
	const divStyles = {
		color: "blue",
		backgroundColor: "grey"
	};
	
	return (
		<div style={divStyles}>
			<div className="infobox">
				<h1>Sale!</h1>
				<p>
					We have a 50% sale going on at https://somewebsite.org. 
					Check it out!
				</p>
			</div>
			<div className="photos">
				<img src="photo1.png">
				<img src="photo2.png">
				...
			</div>
			...
		</div>
	)
}
```

On first glance, this looks like it does more than it needs to. I can clearly see an infobox and a picture gallery. Because of this, we can seperate the infobox into its own component:

```javascript
import React from "react";

import Infobox from "../components/Infobox";

const ExampleComponent = () => {
	const divStyles = {
		color: "blue",
		backgroundColor: "grey"
	};
	
	const infobox = {
		title: "Sale!",
		content: "We have a 50% sale going on at https://somewebsite.org."
			+ "Check it out!"
	};
	
	return (
		<div style={divStyles}>
			<Infobox {...infobox} />
			<div className="photos">
				<img src="photo1.png">
				<img src="photo2.png">
				...
			</div>
			...
		</div>
	)
}
```

After that, we can seperate the `divStyles` from the code by including it as an external CSS file:

```javascript
import React from "react";

import Infobox from "../components/Infobox";

import "../styles/about_page.css";

const ExampleComponent = () => {
	const infobox = {
		title: "Sale!",
		content: "We have a 50% sale going on at https://somewebsite.org."
			+ "Check it out!"
	};
	
	return (
		<div className="page__about">
			<Infobox {...infobox} />
			<div className="photos">
				<img src="photo1.png">
				<img src="photo2.png">
				...
			</div>
			...
		</div>
	)
}
```

Now, because we know so much about the component, we can rename the component and the CSS file:

```javascript
import React from "react";

import Infobox from "../components/Infobox";

import "../styles/AboutPage.css";

const AboutPage = () => {
	const infobox = {
		title: "Sale!",
		content: "We have a 50% sale going on at https://somewebsite.org."
			+ "Check it out!"
	};
	
	return (
		<div className="page__about">
			<Infobox {...infobox} />
			<div className="photos">
				<img src="photo1.png">
				<img src="photo2.png">
				...
			</div>
			...
		</div>
	)
}
```

While we're at it, why not refactor the gallery into a seperate component too? Here's our final finished product:

```javascript
import React from "react";

import Infobox from "../components/Infobox";

import "../styles/AboutPage.css";

const AboutPage = () => {
	const infobox = {
		title: "Sale!",
		content: "We have a 50% sale going on at https://somewebsite.org."
			+ "Check it out!"
	};
	const photos = [
		"photo1.png",
		"photo2.png",
		...
	]
	
	return (
		<div className="page__about">
			<Infobox {...infobox} />
			<Gallery photos={photos} />
			...
		</div>
	)
}
```

Now out code is way easier to understand. While there's a few exceptions to the scroll rule, I always try to adhere to it as much as possible.

# Tip 2: Visualization

Let's say you have a component that is a picture with an optional caption and cite. That's a pretty good way to visualize the component. Because of this, we can construct the `Picture` component like so:

```javascript
const Picture = (props) => {
	return (
		<div className="picture">
			<img src={props.src}>

			<Caption text={props.caption} />
			<Cite text={props.cite} />
		</div>
	)
}
```

This seems like reasonable code. It's simple to understand and functions correctly. This is an example of what I call **visualization**.

Visualization is basically when you get a  component and draw imaginary boxes around each of its smaller components. If you can't possibly draw any more boxes, you know that these will be the base components for the component you're making.

In my example, I know that a caption is most often seen *below* a picture and not *on* a picture. This allows me to know to seperate the caption from the picture.

That's it for today's post, stay tuned for more!

