# react-amp-components
[![npm version](https://badge.fury.io/js/react-amp-components.svg)](https://badge.fury.io/js/react-amp-components)

A (hopefully) simple way to render AMP components via React on the server. 

It uses [react-helmet](https://github.com/nfl/react-helmet) for managing all the required meta and script tags.

That means that you can just import and use the AMP component you need, without having to care about adding the additional required script tags.

### Warning

This project is still in its early infancy. Only one component exists, but we will start populating it with more. Feel free to contribute.

## Installation

Install it from npm:

```bash
yarn add react-amp-components
```
or:
```bash
npm install --save react-amp-components
```

## Usage

Import and use the AMP component you would like to use:

```javascript static
// MyComponent.js
import { Layout, Sidebar } from 'react-amp-components'

class MyComponent extends React.Component {
  render() {
    <div>
      <Layout>
        <title>{title}</title>
        <link rel="canonical" href="http://canonical.url" />
        <script type="application/ld+json">{`
          {
            "@context": "http://schema.org",
            "@type": "Product"
          }
        `}</script>
      </Layout>
      <Sidebar id="my-sidebar" layout="hidden">
        <p>Here is some sidebar content</p>
      </Sidebar>
    </div>
  }
}
```

Then on the server you generate the final HTML output, and the content for the head:

```javascript static
// Server.js
const React = require('react')
const ReactDOMServer = require('react-dom/server')
const { Helmet } = require('react-amp-components')

const MyComponent = require('./MyComponent.js')

const content = ReactDOMServer.renderToStaticMarkup(
  React.createElement(MyComponent)
)
const meta = Helmet.renderStaticClean()
```

Which you can then interpolate:
```javascript static
const finalHTML = `
  <!doctype html>
  <html ${meta.htmlAttributes}>
    <head>
      ${meta.title}
      ${meta.meta}
      ${meta.link}
      ${meta.script}
      ${meta.noscript}
      ${meta.style}
    </head>
    <body ${meta.bodyAttributes}>
      ${content}
    </body>
  </html>
`
```

## Todo
- [ ] Rename `Layout` to `Helmet`
- [ ] Provide helper `finalHTML` renderer
- [ ] Document which components are implemented