# Converting code components to Sketch

![Example image](/overview-image.png "Example image")

A quick and dirty experiment and example of creating Sketch components programmatically from HTML code components via [html-sketchapp-cli](https://github.com/seek-oss/html-sketchapp-cli).


## High level workflow
1. Create some code components (tech agnostic)
2. Add some data attributes to the markup
3. Use html-sketchapp-cli to create a JSON representation of the code components
4. Read the JSON file in Sketch via the [Almost Sketch to Sketch plugin](https://github.com/brainly/html-sketchapp/releases/download/v4.1.0/asketch2sketch-4-1-0.sketchplugin.zip) in Sketch.

## Setup

1. Install [node.js and npm](https://nodejs.org/en/)

2. Install [html-sketchapp-cli](https://github.com/seek-oss/html-sketchapp-cli) in your Terminal like this:
  ```
  npm install --global html-sketchapp-cli
  ```
  ```
  html-sketchapp install
  ```

  This will create a html-sketchapp Terminal command, which will to convert code to JSON files, readable by Sketch via a plugin:

3. Install the [Almost Sketch to Sketch plugin](https://github.com/brainly/html-sketchapp/releases/download/v4.1.0/asketch2sketch-4-1-0.sketchplugin.zip) in Sketch, to be able to ready in the JSON files outputted by html-sketchapp

4. You will need some code components marked up with data attributes like to tell the tool how to create the Sketch assets. Colours, text styles and symbols (nested too) are supported at the moment:
 - Create a symbol: ```data-sketch-symbol="buttons/basic"```
 - Create an instance of a symbol: ```data-sketch-symbol-instance="buttons/basic"```
 - Create a text style: ```data-sketch-text="Header/H2"```
 - Create a document colour: ```data-sketch-color="#cc0022"```


The example-component.html file in this repo has marked up examples, see more info [here](https://github.com/seek-oss/html-sketchapp-cli#page-setup).

Once you have all the above installed, have a working 'html-sketchapp' command in your Terminal and some marked up code you are good to go.

## Usage

Run the following command in the Terminal:
```
html-sketchapp  --file ./example-components.html --out-dir output
```
This will generate 2 JSON files in the /output forlder of the current working directory. It does this by launching [Puppeteer](https://github.com/GoogleChrome/puppeteer) and using html-sketchapp underneath to transform the HTML nodes into Sketch layers.

Now open Sketch and go to Plugins > From *Almost* Sketch to Sketch > Import file(s)... and select the 2 JSON files in the /output folder. Sketch will read in the files and generate Symbols, layers, document colours and text styles.

It is NOT perfect, not all [CSS stuff is supported](https://github.com/brainly/html-sketchapp/wiki/What's-supported%3F) but it's still pretty impressive and a tool like this can be used to keep design tokens in sync with WAY less effort as demonstrated by the SEEK design ops team with [their style guide](https://github.com/seek-oss/seek-style-guide) or the React flavour at [Airbnb](https://github.com/airbnb/react-sketchapp)

## More (advanced) usage

You can generate desktop and mobile versions of each symbol (namespaced accordingly) with defining viewports:
```
html-sketchapp --viewports.Desktop 1280x900 --viewports.Mobile 400x568 --file ./example-components.html --out-dir output
```

In more advanced/realistic scenarios JS can be executed and low level html-sketchapp APIs can be used on each converted layer/symbol, to have more fine grained control over naming re-sizing etc. via [Middlewares](https://github.com/seek-oss/html-sketchapp-cli#middleware)
