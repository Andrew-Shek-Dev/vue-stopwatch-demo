[] Bonus : Vue.js
* Why use Vue.js?
* Installation - `npm init vue@latest` and `yarn install`
* VSCode Extension - Vue Language Features (Volar)
* Try : `yarn dev`
* Declarative Rendering(<template></template> that extends HTML, we can describe how the HTML should look like based on JavaScript state. When the state changes, the HTML updates automatically) and Reactivity(Status Management)
* Single-File Component (SCF) 
    ```vue.js
    <template></template>
    <script setup lang="ts"></script>
    <style scoped></style>
    ```
    

* Options API (v2) vs Composition API (v3) and why use Composition API?
* Composition API - we define a component's logic using imported API functions
* start with 
    ```vue.js
    <script setup ...></script>
    ```

* Template
    * double curly braces {{ }} - Mustache
    * Directives(such as v-html and v-bind) which to reactively apply updates to the DOM when the value of its expression changes, of example, v-if,
    * ```<p v-if="seen">Now you see me</p>```
    * ```<a v-bind:href="url"> ... </a>```, argument
    * ```<a v-on:click="doSomething"></a>``` => ```<a @click="doSomething"> ... </a>```
    * ```<a v-bind:[attributeName]="url">...</a>``` => ```<a :[attributeName]="url">...</a>```, Dynamic Arguments
    * ```<a v-on:[eventName]="doSomething"> ... </a>``` => ```<a @[eventName]="doSomething">...</a>```, Dynamic Arguments
    * Note: space and quotes are not allow => computed property
    * Note: avoid naming keys with uppercase characters, browser force to lowercase (not SFC case)
    * ```<form (v-on:)@submit.prevent="onSubmit"></form>```, modifiers such as .prevent

* Javascript expression inside {{ }} AND attribute value of any Vue directives (`v-xxx`)
    * ```<div :id="`list-${id}`"></div>```
    *  {{ number + 1 }}

* {{ }} - one single expression such as ? : AND calling function
* Reactive
  - Vue is able to track the property access and mutations of a reactive object
     ```vue
    <template>
        <p v-if="state.seen" @click="add">Now you see me {{ state.count }}</p>
    </template>
    <script setup lang="ts">
        //Top-level imports and variables declared in <script setup> are automatically usable in the template of the same component.
        import { reactive } from 'vue';
        const state:{seen:boolean,count:number} = reactive({seen:true,count:0})
        function add(){
            state.count++;
        }
    </script>
    <style scoped>
    </style>
    ```
  - Deep Reactivity
    ```vue
    <template>
        <p v-if="state.seen" @click="add">Now you see me {{ state.nested.count }}</p>
    </template>
    <script setup lang="ts">
        import { reactive } from 'vue';
        const state:{seen:boolean,nested:{count:number}} = reactive({seen:true, nested:{count:0}})
        function add(){
            state.nested.count++;
        }
    </script>
    <style scoped>
    </style>
    ```

  - Only the proxy is reactive `reactive()` - mutating the original object will not trigger updates. Therefore, the best practice when working with Vue's reactivity system is to exclusively use the proxied versions of your state.

    ```vue
    const raw = {}
    const proxy = reactive(raw)
    // proxy is NOT equal to the original.
    console.log(proxy === raw) // false
    ```

  - It only works for object types, not primitive types 
  - calling reactive() will lose reference of the first one
    ```vue
    const state = reactive({ count: 0 })
    // n is a local variable that is disconnected
    // from state.count.
    let n = state.count
    // does not affect original state
    n++

    // count is also disconnected from state.count.
    let { count } = state
    // does not affect original state
    count++

    // the function receives a plain number and
    // won't be able to track changes to state.count
    callSomeFunction(state.count)
    ```

  - To resolve these limitations, `ref` is used here, so that it allows us to create reactive "refs" that can hold any value type:
    ```vue
    <template>
        <p v-if="state.seen" @click="add">Now you see me {{ state.nested.count }}</p>
    </template>
    <script setup lang="ts">
        import {ref, type Ref} from "vue";
        const state:Ref<{seen:boolean,nested:{count:number}}> = ref({seen:true, nested:{count:0}});
        function add(){
            state.value.nested.count++;
        }
        const objectRef = ref({ count: 0 })
        // this works reactively
        objectRef.value = { count: 1 }
    </script>
    <style scoped>
    </style>
    ```
* lifecycle
* socket.io.client
* plugin