# Notes on Gatsby

* Gatsby is a static site generator that uses React and GraphQL
* Part of a JAM stack (Javascript, API's, Markup)


## Using scss modules on Gatsby

You will need to install a few things:

#### `npm i node-sass gatsby-plugin-sass`

Then, create/edit the `gatsby-config.js` file and add the following
```javascript
plugins: [
    // plugins ...
    'gatsby-plugin-sass'
]
```
Thats it! you can start using SCSS Modules now! :smile: 

## Querying Data with GraphQL

**GraphQL**
* Fetch data in a declarative way
* From databases, fyle system, etc
* Via plugins

2 Types of GraphQL Queries on Gatsby:
1. **Page Queries**: runs inside a page components
2. **Static Queries**: runs inside non-page components such as header, layout, etc components.

## Adding site metadata
Stored in the form of JS object inside the `gatsby-config.js` file

```javascript
module.exports = {
    siteMetadata:{
        title: 'My Cool Site',
        description: 'A site that is lit beyong measure',
        author: 'Hugo Perdomo',
    },
    plugins: [...],
}
```

## GraphiQL
In browser IDE to explore GraphQL data sources

Example:
```graphql
{
    site {
        siteMetadata{
            title
        }
    }
}
```

## Page Component Queries
Define the query just like you would in GraphiQL and assume that it will be passed to the page component as a prop named `data`.
```javascript
/**
 * about.js or some page
 */

// The result of this query will be passed along into a prop called data
export const query = graphql`query{
    site {
        siteMetadata {
            author
        }
    }
}`;

// Some component that uses the data
export default ({data}) => (
    <BlogPost author={data.site.siteMetadata.author}/>
);
```

## Static Queries

> Note that will be importing `StaticQuery` and `graphql` from `'gatsby'`

```javascript
/**
 * header.js
 */

import {..., StaticQuery, graphql} from 'gatsby' // Absolutely neccessary

<StaticQuery 
    query = {graphql`
        query {
            site {
                siteMetadata {
                    author
                }
            }
        }
    `} 
    render = {data => (
        // render something with the data
    )} 
/>
```

## Extending Gatsby

A couple of use cases:
* Extend core functionality
* Fetch content and data
* Add third part services (google analytics)
  
### Source Plugins
Fetch data from their source
* CMS
* Local File
* API

Example: 

`gatsby-source-filesystem` allows you to read local files from filesystem. To use it you need to add the following to the `plugins` array inside your `gatsby-config.js``
```javascript
{
    resolve: 'gatsby-source-filesystem',
    options: {
        name: 'files',
        path: `${__dirname}/src/pages` // from where to fetch the files
    }
}
```

With your gatsby site started you can then head over to GraphiQL and run the following command:
```javascript
{
    allFile {
        edges{
            node{
                name
                ext
                size
                id
            }
        }
    }
}
```
This query will show you the list of files inside `/src/pages/`.

## Markdown in Gatsby

A blog post in Gatsby could look like this:
```
---
title: "Getting started with Gatsby"
date: "2019-03-07"
image: "https://source.unsplash.com/150x150/?welcome"
keyworks: "blog"
---

# Time to get serious about Gatsby

Lorem ipsum dolor sit amet, consectetur adipiscing elit. 
Pellentesque vitae lobortis tortor. 
Ut orci elit, facilisis in volutpat ut, rhoncus vel tortor. 
Donec sit amet scelerisque elit, vulputate eleifend purus. 
```
> **NOTE**: make sure to go back to your `gatsby-config.js` to change the path of the plugin that will read this markdown file you have just created

### Transformers, time to transform some data :rocket: 

You will need the following plugin:

#### `npm i gatsby-transformer-remark`

Next add it to the plugins array inside `gatsby-config.js`
```javascript
plugins: [
    {...},
    'gatsby-transformer-remark',
]
```

Lastly, the query would look something like this:
```javascript
{
    allMarkdownRemark(sort: {fields: [formatter___date], order: DESC}) {
        totalCount
        edges {
            node {
                id
                frontmatter {
                    title
                    image
                    date(formatString: "MMMM YYYY")
                }
                excerpt
            }
        }
    }
}
```

## Programmatic Page Creation in Gatsby

Dynamic pages in the context of Gatsby mean a page that is generated at build time, into a static HTML file from data coming from markdown files.

### Gatsby API

**onCreateNode**—This event is fired when a node(data object) is created.

**createNodeField**—Create additional fields on existing nodes. Use it to create friendly URLs.

### gatsby-node.js
To write functionality needed to hook into Gatsby's build pipeline. 

Example:
```javascript
const { createFilePath } = require('gatsby-source-filesystem')
exports.onCreateNode = ({ node, getNode, actions }) => {
    if(node.internal.type === 'MarkdownRemark') {
        // Allows to create additional fields on the MD remark nodes
        const { createNodeField } = actions; 
        const slug = createFilePath({node, getNode, basePath: 'markdown'});
        createNodeField({
            node,
            name: 'slug',
            value: slug,
        })
    }
}
```
Next, you will access the data inside the `node` object like this:
```
allMarkdownRemark(...) {
    edges {
        node {
            fields {
                slug
            }
        }
    }
}
```