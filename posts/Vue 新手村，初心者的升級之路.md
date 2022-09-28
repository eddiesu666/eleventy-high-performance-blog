---
title: Vue 新手村，初心者的升級之路
description: 使用 Vue.js 邁出第一步
date: 2022-09-28
tags:
    - Microsoft Learn
    - Vue.js
layout: layouts/post.njk
image: /img/posts/microsoft-learn-vue.png
---

### 前言
Vue.js 是當今(2022 年)的前端三大框架(Vue.js、Angular、React)之一。Vue.js 是漸進式的網頁架構，新增 script 指令碼標籤即可讓開發人員快速啟動並執行，也可以使用 Vue CLI 來建立可調整規模的大型應用程式。[Microsoft Learn](https://learn.microsoft.com/zh-tw/) 網站，有個「[使用 Vue.js 邁出第一步](https://learn.microsoft.com/zh-tw/training/paths/vue-first-steps/)」教學，透過實際動手做教你開始用 Vue.js 寫網頁。

### 課程筆記
1. [開始使用 Vue](https://learn.microsoft.com/zh-tw/training/modules/vue-get-started/)
* 若要使用 CDN 將 Vue 新增至網頁，請將下列 `script` 元素新增至您的網頁。
```html
<script src="https://unpkg.com/vue@next"></script>
```

* 若要建立 Vue 應用程式，請叫用 `createApp()` 方法。物件內 `data() `所傳回的任何屬性都是動態的。 Vue.js 會自動偵測任何值變更。若要在網頁上顯示資料，請使用 {{ }} 語法，有時也稱為 handlebars。
```html
<!-- the HTML element which will host our app -->
<div id='app'>
    {{ lastName }}, {{ firstName }}
</div>

<script src="https://unpkg.com/vue@next"></script>
<script>
    const App = Vue.createApp({
        data() {
            return {
                firstName: 'Christopher',
                lastName: 'Harrison'
            };
        }
    });
    // Registering and mounting our app
    App.mount('#app');
</script>
```

* 指示詞是 Vue.js 可辨識的屬性。 其可讓您以動態方式設定 HTML 屬性的值。 所有指示詞的開頭都是 v-。核心指示詞為 `v-bind`。 其可讓您將資料值繫結至屬性。 例如：使用 `v-bind:src="value"`,也可以在速記中鍵入此指示詞，例如：`:src="value"`。
```html
<div id="app">
    <img v-bind:src="imageSource" />
    <img :src="imageSource" />
</div>

<script src="https://unpkg.com/vue@next"></script>
<script>
    Vue.createApp({
        data() {
            return {
                imageSource: './media/sample.jpg'
            }
        }
    }).mount('#app');
</script>
```

*  Vue 不僅允許對字串進行繫結，也允許物件的繫結。以下是 `class` 可以將不同屬性的靜態值 `centered active` 切換移出的方式，資料屬性 classObject 有兩個屬性，其值為布林值。 布林值可讓您啟用或停用特定類別。 也可以建立樣式物件來設定 `style`。
```html
<div id="app">
    <div :class="classObject" :style="styleObject">Hello, Vue!</div>
</div>

<script src="https://unpkg.com/vue@next"></script>
<script>
    Vue.createApp({
        data() {
            return {
                classObject: {
                    centered: true,
                    active: true
                },
                styleObject: {
                    'background-color': 'red'
                }
            }
        }
    }).mount('#app');
</script>
```

2. [使用 Vue.js 的動態頁面顯示](https://learn.microsoft.com/zh-tw/training/modules/vue-dynamic-rendering/)
* 範例
```bash
git clone https://github.com/MicrosoftDocs/mslearn-vue-dynamic-render/
```
* 若要顯示清單中的所有項目，您可以使用指示詞 `v-for`。 `v-for` 的行為與 JavaScript 中的 `for...of` 迴圈很類似。 它會逐一查看集合，並透過您所宣告的變數來提供每個項目的存取權。
```html
<ul id="demo">
    <li v-for="(name, index) in names" :key="index">{{ name }}</div>
</ul>

<script src="https://unpkg.com/vue@next"></script>
<script>
    Vue.createApp({
        data() {
            return {
                names: ['Susan', 'Peter', 'Bill' ]
            }
        }
    }).mount('#demo');
</script>
```

* Vue.js 支援三個指示詞：`v-if`、`v-else-if` 和 `v-else`。 如您所預期，這些都完全符合傳統的 `if`、`else if` 和 `else` 陳述式。
```html
<div v-if="new Date().getMonth() < 3">It is the first quarter</div>
<div v-else-if="new Date().getMonth() < 6">It is the second quarter</div>
<div v-else-if="new Date().getMonth() < 9">It is the third quarter</div>
<div v-else>It is the fourth quarter</div>
```

3. [在 Vue.js 中使用資料與事件](https://learn.microsoft.com/zh-tw/training/modules/vue-data-events/)
* 範例
```bash
git clone https://github.com/MicrosoftDocs/mslearn-vue-data-events
```

* `v-model` 指示詞會在 HTML 控制項和相關聯的資料之間建立「雙向」繫結。`v-bind` 指示詞只會建立單向繫結。
```html
<div id="app">
    <!--若要與文字方塊繫結，請使用指示詞 v-model。每當文字方塊的值變更時，name 屬性便會更新。 如果您想要改用 textarea，語法是相同的。-->
    <input type="text" v-model="name" />

    <!--通常，布林值會與核取方塊繫結。 核取方塊能允許選項切換。-->
    <input type="checkbox" v-model="active" /> Is active

    <!--您可能會有兩個選擇，例如「是」和「否」。 在此情況下，您可以使用 true-value 和 false-value 來指示所選核取方塊為已勾選 (true) 或未勾選 (false)。-->
    <input type="checkbox" v-model="benefitsSelected" true-value="yes" false-value="no"> Benefits selected: {{ benefitsSelected }}

    <!--若要建立由選項所組成的清單，請使用 v-for 對清單執行迴圈。 然後選擇將值設定為陣列中項目的索引。 您可以使用 v-for(status, index) in statusList 來為每個項目提供索引。 然後您可以將每個選項的 :value 設為 index，並顯示 status 為使用者的選項。-->
    <select v-model="statusIndex">
    <!-- Create a message to select one -->
    <option disabled value="-1">Please select one</option>
    <!-- Use v-for to create the list of options -->
    <option v-for="(status, index) in statusList" :value="index">
        {{ status }}
    </option>
</select>
</div>

<script src="https://unpkg.com/vue@next"></script>
<script>
    Vue.createApp({
        data() {
            return {
                name: 'Cheryl',
                statusIndex: -1,
                active: true,
                benefitsSelected: 'yes',
                statusList: [
                    'full-time',
                    'part-time',
                    'contractor'
                ]
            }
    }
    }).mount('#app');
</script>
```

* `v-show`，可讓您在 Vue.js 中切換項目的顯示畫面。 並可加上 ! 來反轉。
```html
<form v-show="!booking.completed">
```

* TODO 註解是一種註記未完成工作的好方法。
```html
<!--TODO: 未完成... -->
<script>
// TODO: 未完成...
</script>
```

* Vue.js 會提供一個稱為 `v-on` 的指示詞，您可以將其與任何事件繫結，例如 `v-on:click`。 由於處理事件是核心工作，Vue.js 也會提供所有事件的 @ 捷徑。 因此若要繫結點擊事件，您可以使用 `@click` 捷徑。可以藉由將函式新增至 `methods` 欄位，在 Vue 應用程式或元件中建立事件處理常式，它會維護應用程式的可用函式清單，而不是傳回狀態物件。將函式新增至 methods 欄位的主要原因是，函式可以存取任何已註冊的資料。
```html
<div id="app">
    <button type="button" @click="displayName">Display name</button>
</div>

<script src="https://unpkg.com/vue@next"></script>
<script>
    Vue.createApp({
        data() {
            return {
                name: 'Cheryl'
            }
        },
        methods: {
            displayName() {
                console.log(this.name);
            }
        }
    }).mount('#app');
</script>
```

* 如同將方法新增在 `methods` 欄位下，計算屬性也是以同樣的方式新增在 `computed` 欄位下。 「計算屬性」 是傳回值的函式。 如同方法，計算屬性可以利用 `this` 來存取 Vue 的使用中執行個體。
```html
<div id="app">
    <div>{{ fullName }} </div>
</div>

<script src="https://unpkg.com/vue@next"></script>
<script>
    Vue.createApp({
        data() {
            return {
                firstName: 'Cheryl',
                lastName: 'Smith'
            }
        },
        computed: {
            fullName() {
                return `${this.lastName}, ${this.firstName}`
            }
        },
    }).mount('#app');
</script>
```

4. [開始使用 Vue.js 中的 Vue CLI 和單一檔案元件](https://learn.microsoft.com/zh-tw/training/modules/vue-cli-components/)
* 範例
```bash
git clone https://github.com/MicrosoftDocs/mslearn-vue-components
```

* Vue CLI 的核心用法是启动应用程序。 create 指令碼會提供一個精靈，讓您從中選取一些最常用的設定，包括：
> - Lint 分析選項：確定所有程式碼看起來都一致。 這些選項也有助於找出錯誤。
> - 應用程式類型：選擇是否要新增漸進式 Web 應用程式支援。
> - Babel 支援：如果您的應用程式需要在較舊的瀏覽器中使用，Babel 的作用就是將較新的 JavaScript 語法轉換為較舊的 JavaScript 格式。
> - 語言：選擇 JavaScript 或 TypeScript。 兩個選項都適用，但 TypeScript 的功能之一是可提供類型，因此在應用程式成長時可能是不錯的選擇。 Vue 本身內建於 TypeScript 中。

* 安裝 Node.js，用 `npm` 安裝 Vue CLI
```bash
npm install -g @vue/cli
```

* 建立新專案
```bash
vue create {projectName}
```

* 執行開發伺服器
```bash
npm run serve
```

* 檔案結構：package.json、public/index.html、src/main.js、src/App.vue、src/components

* Vue 元件包含三個主要區段：`style`、`script` 和 `template`。
> - `style` 區段可包含任何有效的 CSS，或您可能使用的任何前置處理器的語法。
> - `script` 區段會儲存用於元件的指令碼。 如同 Vue JavaScript 元件，您也可以匯出各種 Vue 屬性和方法，例如 `data()`、`methods` 和 `components`。
> - `template` 區段含有您想要用來顯示資訊，以及讓使用者與資料互動的 HTML 範本。

* 單一檔案元件會以 .vue 副檔名儲存。 您可以使用 `import` 陳述式，以類似的方式將這些元件載入至其他課程模組。 您可以使用 `components` 屬性加以註冊。 元件經註冊後，即可用作 `template` 內的標籤。當您使用 `import` 匯入程式庫時，標準做法是使用 *PascalCase* 來命名，將每個字的第一個字母顯示為大寫 (例如 PascalCase)。 但在 HTML 中，照慣例是讓標籤名稱使用 *kebab-case*：每個字皆為小寫字母，並且以虛線 (-) 連接。 Vue 會自動管理這兩種不同的慣例。
```html
<template>
<product-display></product-display>
</template>
<script>
import ProductDisplay from './ProductDisplay.vue'
export default {
    components: {
        ProductDisplay
    }
}
</script>
```

* 單一檔案元件可讓您使用 `src` 屬性為 `script` 和 `style` 區段建立個別的檔案。
```html
<template>
    <div>Hello, world</div>
</template>
<script src="./hello.js"></script>
<style src="./style.css"></style>
```

* 安裝 Visual Studio Code 延伸模組
> - [Vetur](https://marketplace.visualstudio.com/items/?itemName=octref.vetur) 可讓您在 Visual Studio Code 中支援 .vue 檔案。
> - 來自 Sarah Drasner 的 [Vue VSCode Snippets](https://marketplace.visualstudio.com/items?itemName=sdras.vue-vscode-snippets) 可在 Visual Studio Code 中啟用程式碼片段。

* Vue CLI 會建立 main.js 檔案，而此檔案會載入 App.vue 作為應用程式的進入點。 基於這個目的，我們建立了名為 Host 的新元件。 我們將更新 main.js 以使用我們的元件。
```javascript
import { createApp } from 'vue'
import Host from './components/Host.vue'

createApp(Host).mount('#app')
```

* `props` 是 properties 的簡稱，是一組可傳入元件的值。 您通常會將屬性新增至元件，以傳入元件應顯示的值，或變更其行為。
```html
<!-- UserDisplay component -->
<script>
export default {
    name: 'UserDisplay',
    props: ['name', 'age']
}
</script>
```

```html
<!-- inside parent component -->
<template>
    <!--值 Cheryl 和 28 會透過屬性繫結分別繫結至 name 和 age 屬性。-->
    <user-display name='Cheryl' age='28'></user-display>
</template>
<script>
import UserDisplay from './UserDisplay.vue';
export default {
    components: {
        UserDisplay
    }
}
</script>
```

* `props` 指定型別。
```html
<!-- UserDisplay component script -->
<script>
export default {
    name: 'UserDisplay',
    props: {
        name: String,
        age: Number
    }
}
</script>
```

* `props` 使用物件。
```html
<template>
    <div>Name: {{ user.name }}</div>
    <div>Age: {{ user.age }}</div>
</template>
<!-- UserDisplay component script -->
<script>
export default {
    name: 'UserDisplay',
    props: {
        user: {
            name: String,
            age: Number
        }
    }
}
</script>
```

```html
<!-- parent component -->
<template>
<user-display :user="user"></user-display>
</template>

<script>
import UserInfo from './UserInfo.vue';
export default {
    data() {
        return {
            user: {
                firstName: 'Cheryl',
                age: 28
            }
        }
    },
    components: {
        UserDisplay
    }
}
</script>
```

* 可以在 script 的 emits 欄位中列出元件可能發出的任何事件，藉以註冊這些事件。可以使用 $emit 函式來發出事件。 如果您想要發出 HTML 控制項已直接引發的事件，您可以用內嵌方式執行此作業。
```html
<template>
    <button @click="$emit('userUpdated')">Save user</button>

    <!--發出含有資料的事件-->
    <button @click="$emit('userUpdated', true)">Save user</button>

    <!--有時，您在發出事件之前可能需要執行更多步驟。-->
    <button @click="saveUser">Save user</button>
</template>
<script>
export default {
    name: 'UserDialog',
    emits: ['userUpdated'],
    methods: {
        saveUser() {
            // perform other operations
            this.$emit('userUpdated'); // emits event
        }
    }
}
</script>
```

* 接聽元件所發出的事件，與接聽一般 HTML 控制項所引發的事件相仿。 您通常會在父元件中建立方法，然後使用與 `@click` 或其他事件相同的 `@<event-name>` 語法，將方法連線至事件。 如果事件傳回任何資料，將會以參數的形式傳至函式。
```html
<!--子元件-->
<template>
    <button @click="$emit('userUpdated', true)">Save user</button>
</template>
<script>
export default {
    name: 'UserDialog',
    emits: ['userUpdated']
}
</script>
```

```html
<!--父元件-->
<template>
<user-dialog @user-updated="handleUserUpdated"></user-dialog>
</template>
<script>
import UserDialog from './UserDialog.vue';
export default {
    methods: {
        handleUserUpdated(success) {
            if (success) {
                alert('It worked!!');
            } else {
                alert('Something went wrong');
            }
        }
    },
    components: {
        UserDialog
    }
}
</script>
```

### 總結
跟著做完大概就知道 Vue.js 的架構是怎麼一回事，從透過 CDN 的輕量使用，到建立專用的使用方式都有提到，雖然沒有很深入，但對 Vue.js 也算是有初步的了解。