# STEP5 値のバインド

## 行う内容
上にあるtext boxの値と下にある入力値の文字を同期させる。  
（text boxの値が変わったら、文字を変わるようにする）  
双方向バインド  

## 実装
### jQuery
```diff
{% raw %}
      <div class="row">
-       <input type="text" class="form-control"/>
+       <input type="text" class="form-control" id="input-item"/>
        <button type="button" class="btn-action">追加</button>
      </div>
 
      <div class="tmp-space">
-       入力値 : <span>掃除</span>
+       入力値 : <span id="text-item">___</span>
      </div>
{% endraw %}
```

`on`メソッドで、イベントにバインドする。  
変更があった時に呼ばれる`input`イベントをトリガーとして  
`trigger`メソッドを呼び出すことで、特定のイベントを発火できる。  
画面の初期化のためにイベントを発火させておく。  

```diff
+ 
+     // STEP 5
+     $('#input-item').on('input', function(){
+       $('#text-item').text($(this).val());
+     }).trigger('input');
    </script>
```

### Vue.js
```diff
        data: function(){
          return {
            title: 'Vue.js',
            isSelectdItem: Math.random() > 0.5,
            todos: {
              high: ['掃除', '洗濯', '炊事'],
              normal: ['買い物', '草刈り', 'アイロン'],
              low: ['窓拭き', '振り込み', '家計簿管理']
-           }
+           },
+           inputItem: ''
          }
```

`v-model`で双方向バインディングできる。  
変数と要素を紐付けることができる。  
```diff
        <div class="row">
-         <input type="text" class="form-control"/>
+         <input type="text" class="form-control" v-model="inputItem"/>
          <button type="button" class="btn-action">追加</button>
        </div>
  
        <div class="tmp-space">
-         入力値 : <span>掃除</span>
+         入力値 : <span>{{ inputItem }}</span>
        </div>
```

### React

Reactには`v-model`のようなものは提供していないので、
inputから値を受け取るイベントと、inputに値を渡すバインドをそれぞれ定義する必要がある、  
イベントとバインドはstep5,step6で詳しく  
ステートフック `React.useState`は引数に初期値の値、配列がreturnされ、値とsetter関数が含まれているので、分割代入する。

```diff
        const todos = {
          high : ['掃除', '洗濯', '炊事'],
          normal: ['買い物', '草刈り', 'アイロン'],
          low: ['窓拭き', '振り込み', '家計簿管理']
        };
+       const [inputItem, setInputItem] = React.useState('');
```

```diff
          <div className="container">
            <h1><span>{ title }</span>のサンプル</h1>
 
            <div className="row">
-             <input type="text" className="form-control"/>
+             <input type="text" className="form-control" 
+               value={ inputItem }
+               onChange={ (e) => setInputItem(e.target.value) }
+             />
              <button type="button" className="btn-action">追加</button>
            </div>

            <div className="tmp-space">
-             入力値 : <span>掃除</span>
+             入力値 : <span>{ inputItem }</span>
            </div>
 
            <div>
```

[STEP4へ](step4.md)  
[STEP6へ](step6.md)  
