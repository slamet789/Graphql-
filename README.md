# Graphql
Graphql adalah sebuah konsep baru dalam membangun sebuah API. Graphql(Query Language) dikembangkan oleh Facebook dan diimplementasikan pada sisi server. Meskipun sebuah query language tetapi Graphql ini tidak berhubungan secara langsung dengan database, dengan kata lain GraphQL tidak terbatas untuk database tertentu baik sql ataupun nosql. Posisi Graphql ini berada pada sisi client dan server yang berhubungan / mengakses suatu API. Salah satu tujuan pengembangan bahasa query ini adalah untuk mempermudah komunikasi data antara backend dan frontend/mobile aplikasi.

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

![expressintasll](https://github.com/slamet789/Graphql-/blob/install1/pict/2.jpg)

Selanjutnya kita cek pada folder apakah node_modules dan package.json sudah ada apa belum dengan ```ls```

![cek](https://github.com/slamet789/Graphql-/blob/install1/pict/3.jpg)

Jika sudah ada, kita buat file example.js dan edit dengan nano menggunakan perintah

```nano example.js```

![nano](https://github.com/slamet789/Graphql-/blob/install1/pict/4.jpg)

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

![start](https://github.com/slamet789/Graphql-/blob/install1/pict/5.jpg)


Buka ip host ke browser untuk mengeceknya pada port 3000

Jika muncul tampilan seperti di bawah ini, maka graphql sudah jalan

![cek1](https://github.com/slamet789/Graphql-/blob/install1/pict/6.jpg)

Kita juga bisa mengecek dengan menambahkan perintah

```
{
  blogTitle
}
  ```
Kemudian klik tombol play, maka akan muncul output berbentuk json di bagian kanan, seperti gambar di bawah ini :

![cek2](https://github.com/slamet789/Graphql-/blob/install1/pict/7.jpg)