# STEP9 仮のデータを削除(完成)

## 行う内容
`<div class="tmp-space">`となっている要素の削除  
初期値として入れていたデータの削除

## 実装

### jQuery
```diff
{% raw %}
      </div>
  
-     <div class="tmp-space">
-       入力値 : <span id="text-item">___</span>
-     </div>
-
      <div>
{% endraw %}
```

```diff
{% raw %}
      </div>

-     <div class="tmp-space">
-       重要度の変更を表示 : <span id="text-is-selectd-item">___</span>
-     </div>

      <div id="area-is-selectd-item">
{% endraw %}
```

```diff
{% raw %}
      // STEP 4
      const todos = {
-       high : ['掃除', '洗濯', '炊事'],
-       normal: ['買い物', '草刈り', 'アイロン'],
-       low: ['窓拭き', '振り込み', '家計簿管理']
+       high : [],
+       normal: [],
+       low: []
      };
{% endraw %}
```

### Vue.js
```diff
{% raw %}
       </div>

-      <div class="tmp-space">
-        入力値 : <span>{{ inputItem }}</span>
-      </div>
-
       <div>
{% endraw %}
```

```diff
{% raw %}
       </div>

-      <div class="tmp-space">
-        重要度の変更を表示 : <span>{{ isSelectdItem }}</span>
-      </div>
-
       <div v-if="isSelectdItem">
{% endraw %}
```

```diff
{% raw %}
          return {
            title: 'Vue.js',
            todos: {
-             high: ['掃除', '洗濯', '炊事'],
-             normal: ['買い物', '草刈り', 'アイロン'],
-             low: ['窓拭き', '振り込み', '家計簿管理']
+             high: [],
+             normal: [],
+             low: []
            },
            inputItem: '',
            selectedItem: null
{% endraw %}
```

### React
```diff
{% raw %}
           </div>

-          <div className="tmp-space">
-            入力値 : <span>{ inputItem }</span>
-          </div>
-
           <div>
{% endraw %}
```

```diff
{% raw %}
           </div>

-          <div className="tmp-space">
-            重要度の変更を表示 : <span>{ String(isSelectdItem()) }</span>
-          </div>
-          
           { isSelectdItem() && (
{% endraw %}
```

```diff
{% raw %}
      function App(){
        const [todos, setTodos] = React.useState({
-         high : ['掃除', '洗濯', '炊事'],
-         normal: ['買い物', '草刈り', 'アイロン'],
-         low: ['窓拭き', '振り込み', '家計簿管理']
+         high : [],
+         normal: [],
+         low: []
        });
        const [inputItem, setInputItem] = React.useState('');
        const [selectedItem, setSelectedItem] = React.useState(null);
{% endraw %}
```

[STEP8へ](step8.md)  
[STEP10へ](step10.md)  
