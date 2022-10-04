# Introduction

Create `pinia` store from ES class.

## Install

```shell
npm i @softvisio/vue-store
```

## Usage

```javascript
import Store from "@softvisio/vue-store";

// module
class Module1 extends Store {};

// main store
class MyStore extends Store {
    stateProp1; // pinia reactive state property
    _protectedStateProp1 = "test"; // pinia protected reactive state property with initial value

    // props with predefined value instance of Store becomes pinia modules
    module = Module1;

    // pinia reactive getter
    get getter1() {
        return this.stateProp1 + _protectedStateProp1;
    }

    // setter is just setter, no special behaviour is defined
    set getter1 () {}

    // pinia action
    async action1 (value) {
        this.stateProp1 += value;
    }
};
```

-   All enumerable properties become `pinia` reactive state properties.
-   All properties, which predefined values are `Store` sub-classes become `pinia` modules. Instances are silently created during initialization.
-   All `get` accessors become `pinia` reactive getters, returned values are cached by `pinia` (refer to the `pinia` getters documentation).
-   All methods become `pinia` actions.

### store.$subscribe( callbasck, options? )

-   `callback` <Function\> Called on store state modified:
    -   `mutation` <Object\>
    -   `state` <Object\>
-   `options` <Object\>:
    -   `prepend` <boolean\>
-   Returns: <Function\> Unsubscribe callback.

### store.$watch( fn, callbasck, options? )

-   `fn` <Function\> Function to watch:
    -   `state`
    -   `getters`
-   `callback` <Function\> Called, when `fn` return value will be changed:
    -   `newValue` <any\> New value.
    -   `oldValue` <any\> Old value.
    -   `unwatch` <Function\> Unwatch callback.
-   `options` <Object\>:
    -   `deep` <boolean\> To also detect nested value changes inside Objects, you need to pass in deep: true in the options argument. Note that you don’t need to do so to listen for Array mutations.
    -   `immediate` <boolean\> Passing in immediate: true in the option will trigger the callback immediately with the current value of the expression.
-   Returns: <Function\> Unwatch callback.

Reactively watch fn's return value, and call the callback when the value changes

## Using with Vue3

Store class has `vue3` plugin interface. Install store to your `vue3` application:

```javascript
import MyStore from "@stores/my-store.js";

app.use(MyStore);
```

It registers as `vue3` `$store` global property. You can get access from `vue3` components:

<!-- prettier-ignore -->
```javascript
this.$store.prop1;                 // accees reactive state property
this.$store.prop1 = 1;             // commit state property
this.$store.prop2;                 // accees reactive getter
await this.$store.action1();       // dispatch action

this.$store.module1.prop1;         // accees reactive state property from module1

this.$store.module1.$root.prop1;   // accees root store from module
this.$store.module1.$parent.prop1; // accees parent store from module
```
