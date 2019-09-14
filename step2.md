# STEP2 Hello World

## 行う内容
ライブラリ名を値としている変数を  
「XXXのサンプル」のXXXの部分に表示させる

## 実装
### jQuery
spanタグにid属性を付与し、textを書き換える。
```diff
{% raw %}
    <div class="container">
-     <h1><span>XXX</span>のサンプル</h1>
+     <h1><span id="text-title">___</span>のサンプル</h1>

    <div class="row">
{% endraw %}
```

`$(セレクタ)`変更するElementを指定し、
メソッドを呼び出して、操作する。
```diff
{% raw %}
    <script>
      // TODO ここに記述する
-
+     // STEP 2
+     const title = 'jQuery';
+
+     $('#text-title').text(title);
    </script>
{% endraw %}
```

### Vue.js
Vue.jsの場合、変数はobjectのdataプロパティの部分に指定する
```diff
{% raw %}
    <script>
      // TODO ここに記述する
      const app = new Vue({
-       el: '#app'
+       el: '#app',
+       data: function(){
+         return {
+           title: 'Vue.js'
+         }
+       }
      })
   </script>
{% endraw %}
```

`{{ }}`で囲んだ中身は式となり、値を表示できる
```diff
{% raw %}
      <div class="container">
-       <h1><span>XXX</span>のサンプル</h1>
+       <h1><span>{{ title }}</span>のサンプル</h1>
 
        <div class="row">
{% endraw %}
```

### React
Reactの場合式には`{ }`で囲む
```diff
{% raw %}
    <script type="text/babel">
      // TODO ここに記述する
+     const title = 'React';
+    
      function App(){
        return (
          <div className="container">
-           <h1><span>XXX</span>のサンプル</h1>
+           <h1><span>{ title }</span>のサンプル</h1>
 
            <div className="row">
              <input type="text" className="form-control"/>
{% endraw %}
```

[STEP1へ](step1.md)  
[STEP3へ](step3.md)  
