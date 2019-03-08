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
:weary: 

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
siteMetaData:{
    ...
}
```