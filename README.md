# gatsby-remark-relative-images-v2

Convert image src(s) in markdown to be relative to their node's parent directory. This will help [gatsby-remark-images](https://github.com/gatsbyjs/gatsby/tree/master/packages/gatsby-remark-images) match images outside the node folder. For example, use with NetlifyCMS.

NOTE: This was built for use with NetlifyCMS and should be considered a temporary solution until relative paths are supported. If it works for other use cases than great!

## Install

`npm install --save gatsby-remark-relative-images-v2`

## How to use

```javascript
// gatsby-config.js
plugins: [
  // Add static assets before markdown files
  {
    resolve: 'gatsby-source-filesystem',
    options: {
      path: `${__dirname}/static/uploads`,
      name: 'uploads',
    },
  },
  {
    resolve: 'gatsby-source-filesystem',
    options: {
      path: `${__dirname}/src/pages`,
      name: 'pages',
    },
  },
  {
    resolve: `gatsby-transformer-remark`,
    options: {
      plugins: [
        // gatsby-remark-relative-images-v2 must
        // go before gatsby-remark-images
        {
          resolve: `gatsby-remark-relative-images-v2`,
        },
        {
          resolve: `gatsby-remark-images`,
          options: {
            // It's important to specify the maxWidth (in pixels) of
            // the content container as this plugin uses this as the
            // base for generating different widths of each image.
            maxWidth: 590,
          },
        },
      ],
    },
  },
];
```

### To convert frontmatter images

Use the exported function `fmImagesToRelative` in your `gatsby-node.js`. This takes every node returned by your gatsby-source plugins and converts any absolute paths in markdown frontmatter data into relative paths if a matching file is found.

```js
// gatsby-node.js
const { fmImagesToRelative } = require('gatsby-remark-relative-images-v2');

exports.onCreateNode = ({ node }) => {
  fmImagesToRelative(node);
};
```
