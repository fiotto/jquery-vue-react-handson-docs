# STEP4 ループ処理

## 行う内容

表示されているTODOのボタンを
ループで動的に表示する。

## 実装
### jQuery
```diff
          <tbody>
            <tr>
+             <td id="area-todo-high">
-             <td>
-               <p>
-                 <button type="button" class="btn-todo-high selected">掃除</button>
-               </p>
-               <p>
-                 <button type="button" class="btn-todo-high">洗濯</button>
-               </p>
-               <p>
-                 <button type="button" class="btn-todo-high">炊事</button>
-               </p>
              </td>
+             <td id="area-todo-normal">
-             <td>
-               <p>
-                 <button type="button" class="btn-todo-normal selected">買い物</button>
-               </p>
-               <p>
-                 <button type="button" class="btn-todo-normal">草刈り</button>
-               </p>
-               <p>
-                 <button type="button" class="btn-todo-normal">アイロン</button>
-               </p>
              </td>
+             <td id="area-todo-low">
-             <td>
-               <p>
-                 <button type="button" class="btn-todo-low selected">窓ふき</button>
-               </p>
-               <p>
-                 <button type="button" class="btn-todo-low">振り込み</button>
-               </p>
-               <p>
-                 <button type="button" class="btn-todo-low">家計簿管理</button>
-               </p>
              </td>
            </tr>
          </tbody>
```

jQueryで動的に構成する場合は、
DOMのツリー構造を構成する。

```diff
      isSelectdItem ? $('#area-is-selectd-item').show() : $('#area-is-selectd-item').hide();
+
+     // STEP 4
+     const todos = {
+       high : ['掃除', '洗濯', '炊事'],
+       normal: ['買い物', '草刈り', 'アイロン'],
+       low: ['窓拭き', '振り込み', '家計簿管理']
+     };
+
+     $('#area-todo-high').empty();
+     for(todo of todos.high){
+       $('#area-todo-high').append(
+         $('<p></p>').append(
+           $('<button></button>', {
+             'type': 'button',
+             'class': 'btn-todo-high'
+           }).append(todo)
+         )
+       );
+     }
+
+     $('#area-todo-normal').empty();
+     for(todo of todos.normal){
+       $('#area-todo-normal').append(
+         $('<p></p>').append(
+           $('<button></button>', {
+             'type': 'button',
+             'class': 'btn-todo-normal'
+           }).append(todo)
+         )
+       );
+     }
+
+     $('#area-todo-low').empty();
+     for(todo of todos.low){
+       $('#area-todo-low').append(
+         $('<p></p>').append(
+           $('<button></button>', {
+             'type': 'button',
+             'class': 'btn-todo-low'
+           }).append(todo)
+         )
+       );
+     }
    </script>
```
### Vue.js
変数はdataに追加する
```diff
        data: function(){
          return {
            title: 'Vue.js',
-           isSelectdItem: Math.random() > 0.5
+           isSelectdItem: Math.random() > 0.5,
+           todos: {
+             high: ['掃除', '洗濯', '炊事'],
+             normal: ['買い物', '草刈り', 'アイロン'],
+             low: ['窓拭き', '振り込み', '家計簿管理']
+           }
          }
        }
```

`v-for`構文で`Array`もしくは`Object`のループを行えます  
`v-for`構文の中で`${配列要素のエイリアス} in ${ソースデータの配列}`で表記する  
`v-for`構文でループさせる時に、vue側で判別しやすいようにユニークな要素を`v-bind:key`で指定することが推奨されている
`v-bind`については、STEP7で説明

```diff
              <tr>
                <td>
-                 <p>
-                   <button type="button" class="btn-todo-high selected">掃除</button>
-                 </p>
-                 <p>
-                   <button type="button" class="btn-todo-high">洗濯</button>
-                 </p>
-                 <p>
-                   <button type="button" class="btn-todo-high">炊事</button>
-                 </p>
+                 <p v-for="todo in todos.high" v-bind:key="todo">
+                   <button type="button" class="btn-todo-high">{{ todo }}</button>
+                 </p>
                </td>
                <td>
-                 <p>
-                   <button type="button" class="btn-todo-normal selected">買い物</button>
-                 </p>
-                 <p>
-                   <button type="button" class="btn-todo-normal">草刈り</button>
-                 </p>
-                 <p>
-                   <button type="button" class="btn-todo-normal">アイロン</button>
-                 </p>
+                 <p v-for="todo in todos.normal" v-bind:key="todo">
+                   <button type="button" class="btn-todo-normal">{{ todo }}</button>
+                 </p>
                </td>
                <td>
-                 <p>
-                   <button type="button" class="btn-todo-low selected">窓ふき</button>
-                 </p>
-                 <p>
-                   <button type="button" class="btn-todo-low">振り込み</button>
-                 </p>
-                 <p>
-                   <button type="button" class="btn-todo-low">家計簿管理</button>
-                 </p>
+                 <p v-for="todo in todos.low" v-bind:key="todo">
+                   <button type="button" class="btn-todo-low">{{ todo }}</button>
+                 </p>
                </td>
              </tr>
```

### React
```diff
      function App(){
        const isSelectdItem = Math.random() > 0.5;
+       const todos = {
+         high : ['掃除', '洗濯', '炊事'],
+         normal: ['買い物', '草刈り', 'アイロン'],
+         low: ['窓拭き', '振り込み', '家計簿管理']
+       };
        
        return (
```

ループに関しても、特別な構文を提供していないので、
JavaScriptの構文をを使う。
配列のすべての要素に対して呼び出し、新しい配列を作成する
[Array.prototype.map()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/map)  
を使う。

```diff
                <tbody>
                  <tr>
                    <td>
-                     <p>
-                       <button type="button" className="btn-todo-high selected">掃除</button>
-                     </p>
-                     <p>
-                       <button type="button" className="btn-todo-high">洗濯</button>
-                     </p>
-                     <p>
-                       <button type="button" className="btn-todo-high">炊事</button>
-                     </p>
+                     { todos.high.map((todo) => (
+                       <p key={ todo }>
+                         <button type="button" className="btn-todo-high">{ todo }</button>
+                       </p>
+                     )) }
                    </td>
                    <td>
-                     <p>
-                       <button type="button" className="btn-todo-normal selected">買い物</button>
-                     </p>
-                     <p>
-                       <button type="button" className="btn-todo-normal">草刈り</button>
-                     </p>
-                     <p>
-                       <button type="button" className="btn-todo-normal">アイロン</button>
-                     </p>
+                     { todos.normal.map((todo) => (
+                       <p key={ todo }>
+                         <button type="button" className="btn-todo-normal">{ todo }</button>
+                       </p>
+                     )) }
                    </td>
                    <td>
-                     <p>
-                       <button type="button" className="btn-todo-low selected">窓ふき</button>
-                     </p>
-                     <p>
-                       <button type="button" className="btn-todo-low">振り込み</button>
-                     </p>
-                     <p>
-                       <button type="button" className="btn-todo-low">家計簿管理</button>
-                     </p>
+                     { todos.low.map((todo) => (
+                       <p key={ todo }>
+                         <button type="button" className="btn-todo-low">{ todo }</button>
+                       </p>
+                     ))}
                    </td>
                  </tr>
                </tbody>
```

[STEP3へ](step3.md)  
[STEP5へ](step5.md)  
