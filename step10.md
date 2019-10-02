# STEP10 コンポーネント

## 行う内容
アイテムを選択した時に表示される、  
重要度変更の領域を別のコンポーネントにする。

### コンポーネントとは


## 実装

### jQuery
なし

### Vue.js
Vue.jsのコンポーネント以下の方法でコンポーネントを実装できる
・文字列テンプレート
・x-template　(今回はこちらの方法で実装する)
・単一ファイルコンポーネント

コンポーネントのテンプレートを作成  
「上げる」「下げる」ボタンの判定方法`v-bind:disabled`がことなうので注意する

``` diff
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
```

選択されている重要度を求める`computed`を追加する  
作成する子コンポーネントに送るため
```diff
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
```

コンポーネントの定義
``` diff
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
```

コンポーネントの使用
``` diff
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
```

### React


[STEP9へ](step9.md)  
