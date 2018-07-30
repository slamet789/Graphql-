# Graphql
Graphql adalah sebuah konsep baru dalam membangun sebuah API. Graphql(Query Language) dikembangkan oleh Facebook dan diimplementasikan pada sisi server. Meskipun sebuah query language tetapi Graphql ini tidak berhubungan secara langsung dengan database, dengan kata lain GraphQL tidak terbatas untuk database tertentu baik sql ataupun nosql. Posisi Graphql ini berada pada sisi client dan server yang berhubungan / mengakses suatu API. Salah satu tujuan pengembangan bahasa query ini adalah untuk mempermudah komunikasi data antara backend dan frontend/mobile aplikasi.

# Instal Graphql dan Express

## Pastikan node.js dan npm sudah terinstal
gunakan perintah :

```node -v```

dan

```npm -v```

pastikan node js v6 keatas

## Setting GraphQL dan Express
dengan perintah :

```npm init```
 
 dan
 
 ```npm install express --save```
 
![npminit](https://github.com/slamet789/Graphql-/blob/install1/pict/1.jpg)

## Install express

**Selanjutnya kita install express dengan perintah :**

```npm install express --save```

![expressintasll](https://github.com/slamet789/Graphql-/blob/install2/pict/2.jpg)

Selanjutnya kita cek pada folder apakah node_modules dan package.json sudah ada apa belum dengan ```ls```

![cek](https://github.com/slamet789/Graphql-/blob/install2/pict/3.jpg)

Jika sudah ada, kita buat file example.js dan edit dengan nano menggunakan perintah

```nano example.js```

![nano](https://github.com/slamet789/Graphql-/blob/install2/pict/4.jpg)

tuliskan syntax :

```// example.js
const express = require('express');
const { buildSchema } = require('graphql');
const graphqlHTTP = require('express-graphql');
let port = 3000;

/* Here a simple schema is constructed using the GraphQL schema language (buildSchema). 
   More information can be found in the GraphQL spec release */

let schema = buildSchema(`
  type Query {
    postTitle: String,
    blogTitle: String
  }
`);

// Root provides a resolver function for each API endpoint
let root = {
  postTitle: () => {
    return 'Build a Simple GraphQL Server With Express and NodeJS';
  },
  blogTitle: () => {
    return 'scotch.io';
  }
};

const app = express();
app.use('/', graphqlHTTP({
  schema: schema,
  rootValue: root,
  graphiql: true //Set to false if you don't want graphiql enabled
}));

app.listen(port);
console.log('GraphQL API server running at localhost:'+ port);

```

Selanjutnya kita cek dengan perintah :

```node example.js```

![start](https://github.com/slamet789/Graphql-/blob/install2/pict/5.jpg)


Buka ip host ke browser untuk mengeceknya pada port 3000

Jika muncul tampilan seperti di bawah ini, maka graphql sudah jalan

![cek1](https://github.com/slamet789/Graphql-/blob/install2/pict/6.jpg)

Kita juga bisa mengecek dengan menambahkan perintah

```
{
  blogTitle
}
```

Kemudian klik tombol play, maka akan muncul output berbentuk json di bagian kanan, seperti gambar di bawah ini :

![cek2](https://github.com/slamet789/Graphql-/blob/install2/pict/7.jpg)

# Konfigurasi

Buat folder src, di dalam src buat schema.js

	const Authors = require('./data/authors'); // This is to make available authors.json file
	const Posts = require('./data/posts'); // This is to make available post.json file
	
	/* Here a simple schema is constructed without using the GraphQL query language. 
	  e.g. using 'new GraphQLObjectType' to create an object type 
	*/
	
	let {
	  // These are the basic GraphQL types need in this tutorial
	  GraphQLString,
	  GraphQLList,
	  GraphQLObjectType,
	  // This is used to create required fileds and arguments
	  GraphQLNonNull,
	  // This is the class we need to create the schema
	  GraphQLSchema,
	} = require('graphql');
	
Di dalam src, buat folder data. Di dalam data buat authors.js dan posts.js, file diambil dari:
```https://github.com/codediger/graphql-expressnodejs/tree/master/src/data```

Install lodash   
```npm install lodash --save```

![3](https://github.com/slamet789/Graphql-/blob/config1/pict/8.jpg)

Edit schema.js menjadi seperti di bawah ini.
	
	// schema.js
	const _ = require('lodash');
	
	// Authors and Posts get data from JSON Arrays in the respective files.
	const Authors = require('./data/authors');
	const Posts = require('./data/posts');
	
	/* Here a simple schema is constructed without using the GraphQL query language. 
	  e.g. using 'new GraphQLObjectType' to create an object type 
	*/
	
	let {
	  // These are the basic GraphQL types need in this tutorial
	  GraphQLString,
	  GraphQLList,
	  GraphQLObjectType,
	  // This is used to create required fileds and arguments
	  GraphQLNonNull,
	  // This is the class we need to create the schema
	  GraphQLSchema,
	} = require('graphql');
	
	const AuthorType = new GraphQLObjectType({
	  name: "Author",
	  description: "This represent an author",
	  fields: () => ({
	    id: {type: new GraphQLNonNull(GraphQLString)},
	    name: {type: new GraphQLNonNull(GraphQLString)},
	    twitterHandle: {type: GraphQLString}
	  })
	});
	
	const PostType = new GraphQLObjectType({
	  name: "Post",
	  description: "This represent a Post",
	  fields: () => ({
	    id: {type: new GraphQLNonNull(GraphQLString)},
	    title: {type: new GraphQLNonNull(GraphQLString)},
	    body: {type: GraphQLString},
	    author: {
	      type: AuthorType,
	      resolve: function(post) {
	        return _.find(Authors, a => a.id == post.author_id);
	      }
	    }
	  })
	});
	
	// This is the Root Query
	const BlogQueryRootType = new GraphQLObjectType({
	  name: 'BlogAppSchema',
	  description: "Blog Application Schema Root",
	  fields: () => ({
	    authors: {
	      type: new GraphQLList(AuthorType),
	      description: "List of all Authors",
	      resolve: function() {
	        return Authors
	      }
	    },
	    posts: {
	      type: new GraphQLList(PostType),
	      description: "List of all Posts",
	      resolve: function() {
	        return Posts
	      }
	    }
	  })
	});
	
	// This is the schema declaration
	const BlogAppSchema = new GraphQLSchema({
	  query: BlogQueryRootType
	  // If you need to create or updata a datasource, 
	  // you use mutations. Note:
	  // mutations will not be explored in this post.
	  // mutation: BlogMutationRootType 
	});
	
	module.exports = BlogAppSchema;
