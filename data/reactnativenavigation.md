# React Native Navigation

## Introduction

Navigation is essential for mobile apps. This tutorial covers React Navigation for screen navigation.

---

## Stack Navigation

    import { NavigationContainer } from '@react-navigation/native';
    import { createStackNavigator } from '@react-navigation/stack';
    
    const Stack = createStackNavigator();
    
    function App() {
      return (
        <NavigationContainer>
          <Stack.Navigator>
            <Stack.Screen name="Home" component={HomeScreen} />
            <Stack.Screen name="Details" component={DetailsScreen} />
          </Stack.Navigator>
        </NavigationContainer>
      );
    }

---

## Conclusion

Use React Navigation for navigation in React Native apps. It supports stack, tab, and drawer navigation.

