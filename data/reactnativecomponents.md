# React Native Components Guide

## Introduction

React Native provides built-in components for building mobile UIs. This tutorial covers View, Text, Image, ScrollView, FlatList, and more.

---

## Basic Components

    import { View, Text, StyleSheet } from 'react-native';
    
    function App() {
      return (
        <View style={styles.container}>
          <Text style={styles.text}>Hello React Native</Text>
        </View>
      );
    }
    
    const styles = StyleSheet.create({
      container: { flex: 1, justifyContent: 'center', alignItems: 'center' },
      text: { fontSize: 20 }
    });

---

## Lists

    import { FlatList } from 'react-native';
    
    const data = [{ id: '1', title: 'Item 1' }];
    
    <FlatList
      data={data}
      renderItem={({ item }) => <Text>{item.title}</Text>}
      keyExtractor={item => item.id}
    />

---

## Images

    import { Image } from 'react-native';
    
    <Image
      source={{ uri: 'https://example.com/image.jpg' }}
      style={{ width: 200, height: 200 }}
    />

---

## Conclusion

Master React Native components to build beautiful mobile applications. Use appropriate components for your use case.

