# AsyncStorage in React Native

## Introduction

AsyncStorage provides persistent storage in React Native. This tutorial covers storing and retrieving data.

---

## Basic Usage

    import AsyncStorage from '@react-native-async-storage/async-storage';
    
    // Store
    await AsyncStorage.setItem('key', 'value');
    
    // Retrieve
    const value = await AsyncStorage.getItem('key');
    
    // Remove
    await AsyncStorage.removeItem('key');

---

## Conclusion

Use AsyncStorage for simple key-value storage. For complex data, consider using a database or state management solution.

