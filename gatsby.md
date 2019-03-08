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

## Querying Data with GraphQL`

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
let query = ;
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
    )} />
```