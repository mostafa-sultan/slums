# React Native Styling

## Introduction

Styling in React Native uses StyleSheet API similar to CSS. This tutorial covers styling, Flexbox, and responsive design.

---

## StyleSheet

    import { StyleSheet } from 'react-native';
    
    const styles = StyleSheet.create({
      container: {
        flex: 1,
        backgroundColor: '#fff',
        padding: 20
      }
    });

---

## Flexbox

    const styles = StyleSheet.create({
      container: {
        flex: 1,
        flexDirection: 'row',
        justifyContent: 'space-between',
        alignItems: 'center'
      }
    });

---

## Conclusion

Use StyleSheet for performance and Flexbox for layouts. Create responsive designs that work on different screen sizes.

