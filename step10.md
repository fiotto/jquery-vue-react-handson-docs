# STEP10 コンポーネント

## 行う内容
アイテムを選択した時に表示される、  
重要度変更の領域を別のコンポーネントにする。

### コンポーネントとは
機能ごとにhtmlとJavascript場合によってはCSSをまとめて、
それぞれの部品に切り出した開発するための仕組み。  
切り出しを行なっているので、各領域内でカスタム要素として使用することができる。  
propsの使って、親のコンポーネントから子コンポーネントにデータを渡すことができる。  

## 実装

### jQuery
なし  
やるとしたら`<template>`タグを使って実装

### Vue.js
Vue.jsのコンポーネント以下の方法でコンポーネントを実装できる
・文字列テンプレート
・x-template　(今回はこちらの方法で実装する)
・単一ファイルコンポーネント

コンポーネントのテンプレートを作成する  
「上げる」「下げる」ボタンの判定方法`v-bind:disabled`がことなうので注意する
``` diff
{% raw %}
+   <script type="text/x-template" id="priority">
+     <div v-if="isSelectdItem">
+       <div class="content-center">
+       <div class="card">
+         <p class="text-center">
+         <span class="color-red">{{ selectedItem }}</span>の重要度を
+         </p>
+         <p>
+         <button type="button" class="btn-action"
+           v-bind:disabled="selectedPriority === 'high'"
+           v-on:click="onClickPriority(1)"
+         >上げる</button>
+         <button type="button" class="btn-action"
+           v-bind:disabled="selectedPriority === 'low'"
+           v-on:click="onClickPriority(-1)"
+         >下げる</button>
+         </p>
+       </div>
+       </div>
+     </div>
+   </script>
+ 
    <script>
      // TODO ここに記述する
{% endraw %}
```

現在選択されている重要度を取得するために`computed`を追加する  
作成する子コンポーネントに送るため
```diff
{% raw %}
        },
        computed: {
-         isSelectdItem: function(){
-           return this.selectedItem !== null;
+         selectedPriority: function(){
+           const keys = Object.keys(this.todos);
+           const selectedPriority = keys.filter((a) => this.todos[a].includes(this.selectedItem));
+ 
+           return selectedPriority[0];
          }
        },
        methods: {
{% endraw %}
```

コンポーネントの定義する。（JavaScript部分）  
Vue.jsのpropsはobjectの`props`プロパティで指定できる。  
値の参照には、他と同様に`this.`で取得できる。

`this.$emit('click-priority', signum);`
とすることで、イベントを発火できる。
その際に、引数を指定することで親コンポーネントに値を渡すことができる。    
イベントを受ける側の親コンポーネントは`v-on:`で受け取る

``` diff
{% raw %}
      // TODO ここに記述する
+     Vue.component('priority', {
+       template: '#priority',
+       props: {
+         selectedItem: String,
+         selectedPriority: String
+       },
+       computed: {
+         isSelectdItem: function(){
+           return this.selectedItem !== null;
+         }
+       },
+       methods: {
+         onClickPriority: function(signum){
+           this.$emit('click-priority', signum);
+         }
+       }
+     })
+     
      const app = new Vue({
{% endraw %}
```

コンポーネントの使用  
`<priority>`という独自の要素が使えるようになっているので切り替える  
propsで子コンポーネントに渡す値を`v-bind`  
emitで子コンポーネントから受け取る処理を`v-on`  
それぞれ定義する。

``` diff
{% raw %}
          </table>
        </div>

-       <div v-if="isSelectdItem">
-         <div class="content-center">
-           <div class="card">
-             <p class="text-center">
-               <span class="color-red">{{ selectedItem }}</span>の重要度を
-             </p>
-             <p>
-               <button type="button" class="btn-action"
-                 v-bind:disabled="todos.high.includes(selectedItem)"
-                 v-on:click="onClickPriority(1)"
-               >上げる</button>
-               <button type="button" class="btn-action"
-                 v-bind:disabled="todos.low.includes(selectedItem)"
-                 v-on:click="onClickPriority(-1)"
-               >下げる</button>
-             </p>
-           </div>
-         </div>
-       </div>
+       <priority
+           v-bind:selected-item="selectedItem"
+           v-bind:selected-priority="selectedPriority"
+           v-on:click-priority="onClickPriority"
+       ></priority>

        <div v-if="isSelectdItem">
{% endraw %}
```

### React
Reactの場合functionが一つのコンポーネントになる  
※Class Componentの書き方もある

Reactの場合のpropsは引数から取得する。

コンポーネントにしてしまえば、  
関数なので、条件分岐を論理和演算子を使って実装しなくても、  
if文で表すことができる。  
ループ文も同様。   

Reactでは$emitのようなものは提供していないので、  
`props.onClickPriority(signum)`のように、  
propsにコールバック関数を渡して呼び出す。  

```diff
{% raw %}
+     function Priority(props){
+       function isSelectdItem(){
+         return props.selectedItem !== null;
+       }
+ 
+       function onClickPriority(signum){
+         props.onClickPriority(signum)
+       }
+ 
+       if(isSelectdItem()){
+         return (
+           <div>
+             <div className="content-center">
+               <div className="card">
+                 <p className="text-center">
+                   <span className="color-red">{ props.selectedItem }</span>の重要度を
+                 </p>
+                 <p>
+                   <button type="button" className="btn-action"
+                     disabled={ props.selectedPriority === 'high' }
+                     onClick={ () => onClickPriority(1) }
+                   >上げる</button>
+                   <button type="button" className="btn-action"
+                     disabled={ props.selectedPriority === 'low' }
+                     onClick={ () => onClickPriority(-1) }
+                   >下げる</button>
+                 </p>
+               </div>
+             </div>
+           </div>
+         );
+       }else{
+         return null;
+       }
+     }
+ 
    function App(){
{% endraw %}
```

現在選択されている重要度を取得するための関数
``` diff
{% raw %}
        }
  
-       function isSelectdItem(){
-         return selectedItem !== null;
-       }
+       function selectedPriority(){
+         const keys = Object.keys(todos);
+         const selectedPriority = keys.filter((a) => todos[a].includes(selectedItem));
+
+         return selectedPriority[0];
+       }
 
        function onClickPriority(signum){
{% endraw %}
```

`<Priority>`という独自の要素が使えるようになっているので切り替える  
propsで子コンポーネントに渡す値と  
emitで子コンポーネントから受け取る処理をするコールバック関数    
それぞれ定義する。
```diff
{% raw %}
            </div>
 
-           { isSelectdItem() && (
-             <div>
-               <div className="content-center">
-                 <div className="card">
-                   <p className="text-center">
-                     <span className="color-red">{ selectedItem }</span>の重要度を
-                   </p>
-                   <p>
-                     <button type="button" className="btn-action"
-                       disabled={ todos.high.includes(selectedItem) }
-                       onClick={ () => onClickPriority(1) }
-                     >上げる</button>
-                     <button type="button" className="btn-action"
-                       disabled={ todos.low.includes(selectedItem) }
-                       onClick={ () => onClickPriority(-1) }
-                     >下げる</button>
-                   </p>
-                 </div>
-               </div>
-             </div>
-           )}
+           <Priority
+             selectedItem={ selectedItem }
+             selectedPriority={ selectedPriority() }
+             onClickPriority={ onClickPriority }
+           ></Priority>
          </div>
{% endraw %}
```

[STEP9へ](step9.md)  
