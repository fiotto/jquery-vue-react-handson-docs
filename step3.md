# STEP3 条件分岐

## 行う内容

重要度の領域の表示/非表示が変わるようにする  
このSTEPでは表示させるかどうかは、ランダム値を用いる

## 実装
### jQuery
変化をつけるelementにそれぞれidをつけて、操作する。

```diff
      <div class="tmp-space">
-       重要度の変更を表示 : <span>true</span>
+       重要度の変更を表示 : <span id="text-is-selectd-item">___</span>
      </div>
 
-     <div>
+     <div id="area-is-selectd-item">
        <div class="content-center">
          <div class="card">
            <p class="text-center">
```

DOM自体は消さず表示(show())と非表示(hide())を切り替える
```diff
      $('#text-title').text(title);
+
+     // STEP 3
+     const isSelectdItem = Math.random() > 0.5;
+
+     $('#text-is-selectd-item').text(String(isSelectdItem));
+     isSelectdItem ? $('#area-is-selectd-item').show() : $('#area-is-selectd-item').hide();
```

### Vue.js
```diff
        el: '#app',
        data: function(){
          return {
-           title: 'Vue.js'
+           title: 'Vue.js',
+           isSelectdItem: Math.random() > 0.5
          }
        }
```

`v-if`で式を指定することで、式を評価して、正なら表示、非なら非表示となる  
jQeuryと違い、DOM自体生成されない
```diff
        <div class="tmp-space">
-         重要度の変更を表示 : <span>true</span>
+         重要度の変更を表示 : <span>{{ isSelectdItem }}</span>
        </div>
 
-       <div>
+       <div v-if="isSelectdItem">
          <div class="content-center">
            <div class="card">
```
### React
変数はApp関数（コンポーネント）内で宣言する
```diff
      const title = 'React';
    
      function App(){
+       const isSelectdItem = Math.random() > 0.5;
+     
        return (
          <div className="container">
            <h1><span>{ title }</span>のサンプル</h1>
```

```diff
           <div className="tmp-space">
-            重要度の変更を表示 : <span>true</span>
+            重要度の変更を表示 : <span>{ String(isSelectdItem) }</span>
           </div>
```

Reactの場合ifを行う構文を提供していないので、
`{ }`で式であることを利用して、JavaScript構文で記述  
論理和演算子を用いて、`&&`の前が`true`なら、`&&`の後が評価されることを利用する  
三項演算子で、ifならこのElement、elseならこのElementといった指定も可能
```diff
+           { isSelectdItem && (
              <div>
                <div className="content-center">
                  <div className="card">
```

```diff
                </div>
              </div>
+           )}
          </div>
        );
      }
    
      ReactDOM.render(<App />, document.getElementById('root'));
```

[STEP2へ](step2.md)  
[STEP4へ](step4.md)  
