# React Native
## Table of Contents
- [General](#general)
- [Components](#components)
- [Custom Fonts](#custom-fonts)
- [Stack Navigation](#stack-navigation)
  - [Installation](#installation%3A)
  - [Usage](#usage%3A)
- [Drawer Navigation](#drawer-navigation)
  - [Installation](#installation%3A-2)
  - [Usage](#usage%3A-2)

## General
- Build mobile apps (**Android** & **iOS**) using **React**
- Use the `expo-cli` for project management (create, build, deploy, etc.)
- Instead of rendering **HTML** elements, it renders built-in components that are converted to native **iOS** or **Android** controls
- Can use these built-in components to create custom UI components for full control of the appearance and functionality of the application
- Most components support a `style` property which can be set to a `StyleSheet` object for customizing the component's appearance
- Names of properties in the `StyleSheet` object are in *camelCase* since they are defined in **JavaScript**
- Styles are not inherited by child components; nested `Text` components are the exception

## Components
- **View:** Like `div`, used to wrap other components for styling and organization. Uses flexbox layout by default with `flexDirection: "column"`
- **Text:** Used for displaying text
- **Button:** They have an `onPress` property and a `color` property, but they don't support the `style` property
- **TextInput:** Used for getting user input  
  *`placeholder`* - placeholder text appearing when the input is empty  
  *`multiline`* - set to `true` to allow the text input to contain multiple lines  
  *`keyboardType`* - determines which keyboard to open, e.g. `numeric`  
  *`onChangeText`* - callback that is called when the text input's text changes, with the changed text passed as a single string argument to the callback handler
- **ScrollView:** Useful for adding scrolling capabilities to lists
- **FlatList:** More performant list option with auto scrolling support and auto key lookup for items  
  *`data`* - a plain array of values to display  
  *`renderItem`* - takes an item from data and renders it into the list  
  *`keyExtractor`* - used to extract a unique key for a given item at the specified index  
  *`numColumns`* - render the data in multiple columns
- **TouchableOpacity:** A wrapper for making views respond properly to touches. Provides feedback by dimming them
- **TouchableWithoutFeedback:** Useful when combined with `Keyboard.dismiss`
- **Alert:** Launches an alert dialog with the specified title and message
```javascript
Alert.alert("OOPS!", "Todos must be over 3 chars long", [
  { text: "Understood", onPress: () => console.log("alert closed") }
]);
```

## Custom Fonts
```javascript
import * as Font from "expo-font";
               ...
Font.loadAsync({ "font-name": require("font-path") })
```

## Stack Navigation
### Installation:
1. `npm install @react-navigation/native @react-navigation/native-stack`
2. `npm install react-native-screens react-native-safe-area-context`
   (or `npx expo install` ... for **Expo** managed projects)

### Usage:
```javascript
import * as React from "react";
import { NavigationContainer } from "@react-navigation/native";
import { createNativeStackNavigator } from "@react-navigation/native-stack";
import HomeScreen from "./screens/HomeScreen";
import ProfileScreen from "./screens/ProfileScreen";

const Stack = createNativeStackNavigator();

const App = () => {
  return (
    <NavigationContainer>
        <Stack.Navigator>
            <Stack.Screen
                name="Home"
                component={ HomeScreen }
                options={{ title: "Welcome" }}
            />
            <Stack.Screen name="Profile" component={ProfileScreen} />
        </Stack.Navigator>
    </NavigationContainer>
  );
};

export default App;
```

Each screen component receives a prop called `navigation` which has various methods to link to other screens:

```javascript
const HomeScreen = ({ navigation }) => {
    return (
        <Button
            title="Go to Jane's profile"
            onPress={() => navigation.navigate("Profile", { name: "Jane" })}
        />
    );
};

const ProfileScreen = ({ navigation, route }) => {
    return <Text>This is {route.params.name}'s profile</Text>;
};
```

The header can be customized by using various properties of the `options` prop of `Stack.Screen`:

```javascript
function StackScreen() {
  return (
    <Stack.Navigator>
      <Stack.Screen
        name="Home"
        component={ HomeScreen }
        options={{
          title: "My home",
          headerStyle: {
            backgroundColor: "#f4511e",
          },
          headerTintColor: "#fff",
          headerTitleStyle: {
            fontWeight: "bold",
          },
        }}
      />
    </Stack.Navigator>
  );
}
```

If we want those options to be shared across screens, we can instead move them up to the native stack navigator under the prop `screenOptions`:

```javascript
function StackScreen() {
  return (
    <Stack.Navigator
      screenOptions={{
        headerStyle: {
          backgroundColor: "#f4511e",
        },
        headerTintColor: "#fff",
        headerTitleStyle: {
          fontWeight: "bold",
        },
      }}
    >
      <Stack.Screen
        name="Home"
        component={ HomeScreen }
        options={{ title: "My home" }}
      />
    </Stack.Navigator>
  );
}
```

We can even replace the title with a custom component using the `headerTitle` property of the `options` prop.

## Drawer Navigation
### Installation:
1. `npm install @react-navigation/drawer`
2. `npm install react-native-gesture-handler react-native-reanimated`

### Usage:
```javascript
import "react-native-gesture-handler";
import { NavigationContainer } from "@react-navigation/native";
import { createDrawerNavigator } from "@react-navigation/drawer";
                                ...
const Drawer = createDrawerNavigator();

export default function App() {
    return (
        <NavigationContainer>
            <Drawer.Navigator initialRouteName="Home">
                <Drawer.Screen name="Home" component={ HomeScreen } />
                <Drawer.Screen name="Notifications" component={ NotificationsScreen } />
            </Drawer.Navigator>
        </NavigationContainer>
    );
}
```

We can open the drawer with `navigation.openDrawer()` and close it with `navigation.closeDrawer()`.
