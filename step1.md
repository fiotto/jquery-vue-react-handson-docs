# STEP1 環境の用意

## 行う内容
TemplateのHTMLをVueとReactに移行して操作できるようにする
## 実装
### jQuery
特に変更なし

### Vue.js
対象の領域をdivで囲む
```diff
{% raw %}
  <body>
+   <div id="app">
      <div class="container">
        <h1><span>XXX</span>のサンプル</h1>
{% endraw %}
```

```diff
{% raw %}
        </div>
      </div>
+   </div>

    <script>
      // TODO ここに記述する
- 
+     const app = new Vue({
+       el: '#app'
+     })
    </script>
{% endraw %}
```

### React
- STEP1-1

領域を確保する
```diff
{% raw %}
  <body>
+   <div id="root"></div>
    <div class="container">
      <h1><span>XXX</span>のサンプル</h1>
{% endraw %}
```

```diff
{% raw %}
    <script type="text/babel">
      // TODO ここに記述する
-
+     function App(){
+       return (
+         <div>
+         </div>
+       );
+     }
+    
+     ReactDOM.render(<App />, document.getElementById('root'));
+   </script>
{% endraw %}
```

- STEP1-2

HTMLの要素を移動させる。

```diff
{% raw %}
    <div id="root"></div>
-   <div class="container">
-     <h1><span>XXX</span>のサンプル</h1>
-
-      -- 省略 --
-
-     </div>
-   </div>

    <script type="text/babel">
      // TODO ここに記述する
      function App(){
        return (
+         <div class="container">
+           <h1><span>XXX</span>のサンプル</h1>
+      
+            -- 省略 --
+
+           </div>
+         </div>
-         <div>
-         </div>
        );
      }
{% endraw %}
```

- STEP1-3

JavaScriptの予約語を属性名としているものは、置き換える必要がある  
`class` -> `className`  
`for` -> `htmlFor`

```diff
{% raw %}
+         <div className="container">
-         <div class="container">
{% endraw %}
```

```diff
{% raw %}
+           <div className="row">
-           <div class="row">
+             <input type="text" className="form-control"/>
-             <input type="text" class="form-control"/>
+             <button type="button" className="btn-action">追加</button>
-             <button type="button" class="btn-action">追加</button>
{% endraw %}
```

-- 以下同様 --

[STEP2へ](step2.md)  
