import React from 'react';
import { StyleSheet, View, Text, TouchableOpacity, Dimensions} from 'react-native';
import FastImage from 'react-native-fast-image';

const splashData = {
  background: 'https://i.ibb.co/FKyTQH9/1.png',
  logo: 'https://i.ibb.co/Z1GnkfM/2.png',
  title: 'Welcome in our restaurant app.',
};
const windowWidth = Dimensions.get('window').width;
const loginButtonWidth = windowWidth * 0.8; 

const Index = () => {
  return (
    <View style={styles.container}>
      <FastImage
        style={styles.backgroundImage}
        source={{
          uri: splashData.background,
          priority: FastImage.priority.high,
        }}
        resizeMode={FastImage.resizeMode.cover}
      />
      <View style={styles.contentContainer}>
        <View style={styles.detailsContainer}>
          <FastImage
            style={styles.logoImage}
            source={{ uri: splashData.logo }}
            resizeMode={FastImage.resizeMode.contain} />
          <Text style={styles.title}>{splashData.title}</Text> 
            <TouchableOpacity style={styles.loginButton}
              onPress={() => console.log("Login button pressed")}
            >
              <Text style={styles.loginButtonText}>Start</Text>
            </TouchableOpacity> 
        </View>
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  backgroundImage: {
    ...StyleSheet.absoluteFillObject,
  },
  contentContainer: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  detailsContainer: {
    marginTop:80,
    alignItems: 'center',
  },
  logoImage: {
    aspectRatio: 1.7,
    height: '28%',
  },
  title: {
    fontSize: 18, 
    fontWeight:'bold',
    color:"#000",
  }, 
  loginButton: {
    backgroundColor: 'red',
    width: loginButtonWidth,
    paddingVertical: 13, 
    borderRadius: 8, 
    marginTop:100,
  },
  loginButtonText: {
    color: '#fff',
    fontSize: 16,
    alignSelf: 'center'
  },
});

export default Index;




import React, { useEffect, useRef, useState } from 'react';
import { FlatList, StyleSheet, Text, View, Image, useWindowDimensions, Animated } from 'react-native';
import { ExpandingDot } from "react-native-animated-pagination-dots";

const onBoardingData = [{
  id: 0,
  img: 'https://i.ibb.co/5TfwKGV/Group-14224.png',
  title: 'Welcome in our restaurant app.',
  describtion: "lorimlorim lorimlorim lorimlorim lorimlorim lorimlorim lorimlorim ",
}, {
  id: 1,
  img: 'https://i.ibb.co/3pqZ7mx/Group.png',
  title: 'Welcome in our restaurant app.',
  describtion: "lorimlorim lorimlorim lorimlorim lorimlorim lorimlorim lorimlorim ",
}, {
  id: 2,
  img: 'https://i.ibb.co/dgyDCd6/Group-1.png',
  title: 'Welcome in our restaurant app.',
  describtion: "lorimlorim lorimlorim lorimlorim lorimlorim lorimlorim lorimlorim ",
}];
const SlideComp = ({ item }) => {
  const { width } = useWindowDimensions();
  return (
    <View style={[styles.container1, { width }]}>
      <Image source={{ uri: item.img }} style={[styles.image1, { width, resizeMod: 'contain' }]} />
      {/* source={{ uri: splashData.logo }} */}
      <View style={{ flex: 0.3 }}>
        <Text style={styles.title1}>{item.title}</Text>
        <Text style={styles.describtion1}>{item.describtion}</Text>
      </View>
    </View>

  )
}
const Index = () => {
  const [current, setCurrent] = useState(0)

  scrollX = useRef(new Animated.Value(0)).current

  const slideRef = useRef(null);

  const viewableItemsChanged = useRef(({ viewableItems }) => {
    setCurrent(viewableItems[0]?.index)
  }).current

  const viewconfig = useRef({ viewAreaCoveragePercentThreshold: 50 }).current
  return (
    <View style={styles.container}>
      <Text>Ingrgdex</Text>
      <FlatList
        data={onBoardingData}
        renderItem={({ item }) => <SlideComp item={item} />}
        horizontal
        pagingEnabled
        showsHorizontalScrollIndicator
        bounces={false}
        keyExtractor={(item) => item.id}
        // onScroll={Animated.event([{ nativeEvent: { contentOffset: { x: scrollX } } }], {
        //   useNativeDriver: false
        // })}
        onScroll={Animated.event(
          [{ nativeEvent: { contentOffset: { x: scrollX } } }],
          {
            useNativeDriver: false,
          }
        )}
        onViewableItemsChanged={viewableItemsChanged}
        viewabilityConfig={viewconfig}
        scrollEventThrottle={32}
        ref={slideRef}
      />
      <ExpandingDot
        data={onBoardingData}
        expandingDotWidth={30}
        scrollX={scrollX}
        inActiveDotOpacity={0.6}
        dotStyle={{
          width: 10,
          height: 10,
          backgroundColor: '#347af0',
          borderRadius: 5,
          marginHorizontal: 5
        }}
        containerStyle={{
          marginBottom : 80,
        }}
      />
    </View>
  );
};

export default Index;

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  container1: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  image1: {
    flex: 0.3,
    justifyContent: 'center'
  },
  title1: {
    fontWeight: '800',
    fontSize: 28,
    marginBottom: 10,
    textAlign: 'center',
  },
  describtion1: {
    fontWeight: '300',
    color: '#62656',
    textAlign: 'center',
    paddingHorizontal: 64,
  }
});

// https://www.youtube.com/watch?v=SLbay2efwso

---
title: Absinthe
category: Hidden
layout: 2017/sheet
tags: [WIP]
updated: 2017-10-10
intro: |
  [Absinthe](http://absinthe-graphql.org/) allows you to write GraphQL servers in Elixir.
---
dfgdgf
hi from branch
## Introduction

### Concepts

- `Schema` - The root. Defines what queries you can do, and what types they return.
- `Resolver` - Functions that return data.
- `Type` - A type definition describing the shape of the data you'll return.

### Plug

#### web/router.ex

```elixir
defmodule Blog.Web.Router do
  use Phoenix.Router

  forward "/", Absinthe.Plug,
    schema: Blog.Schema
end
```
{: data-line="4,5"}

Absinthe is a Plug, and you pass it one **Schema**.

See: [Our first query](http://absinthe-graphql.org/tutorial/our-first-query/)

## Main concepts
{: .-three-column}

### Schema

#### web/schema.ex

```elixir
defmodule Blog.Schema do
  use Absinthe.Schema
  import_types Blog.Schema.Types

  query do
    @desc "Get a list of blog posts"
    field :posts, list_of(:post) do
      resolve &Blog.PostResolver.all/2
    end
  end
end
```
{: data-line="5,6,7,8,9,10"}

This schema will account for `{ posts { ··· } }`. It returns a **Type** of `:post`, and delegates to a **Resolver**.

### Resolver

#### web/resolvers/post_resolver.ex

```elixir
defmodule Blog.PostResolver do
  def all(_args, _info) do
    {:ok, Blog.Repo.all(Blog.Post)}
  end
end
```
{: data-line="3"}

This is the function that the schema delegated the `posts` query to.

### Type

#### web/schema/types.ex

```elixir
defmodule Blog.Schema.Types do
  use Absinthe.Schema.Notation

  @desc "A blog post"
  object :post do
    field :id, :id
    field :title, :string
    field :body, :string
  end
end
```
{: data-line="4,5,6,7,8,9"}

This defines a type `:post`, which is used by the resolver.

## Schema

### Query arguments

#### GraphQL query

```
{ user(id: "1") { ··· } }
```

#### web/schema.ex

```elixir
query do
  field :user, type: :user do
    arg :id, non_null(:id)
    resolve &Blog.UserResolver.find/2
  end
end
```
{: data-line="3"}

#### Resolver

```elixir
def find(%{id: id} = args, _info) do
  ···
end
```
{: data-line="1"}

See: [Query arguments](http://absinthe-graphql.org/tutorial/query-arguments/)

### Mutations

#### GraphQL query

```
{
  mutation CreatePost {
    post(title: "Hello") { id }
  }
}
```

#### web/schema.ex

```elixir
mutation do
  @desc "Create a post"
  field :post, type: :post do
    arg :title, non_null(:string)
    resolve &Blog.PostResolver.create/2
  end
end
```
{: data-line="1"}

See: [Mutations](http://absinthe-graphql.org/tutorial/mutations/)

## References

  - [Absinthe website](http://absinthe-graphql.org/) _(absinthe-graphql.org)_
  - [GraphQL cheatsheet](./graphql) _(devhints.io)_
