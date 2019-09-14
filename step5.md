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

```diff
{% raw %}
+ 
+     // STEP 5
+     $('#input-item').on('input', function(){
+       $('#text-item').text($(this).val());
+     }).trigger('input');
    </script>
{% endraw %}
```

### Vue.js
```diff
{% raw %}
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
{% endraw %}
```

```diff
{% raw %}
        <div class="row">
-         <input type="text" class="form-control"/>
+         <input type="text" class="form-control" v-model="inputItem"/>
          <button type="button" class="btn-action">追加</button>
        </div>
  
        <div class="tmp-space">
-         入力値 : <span>掃除</span>
+         入力値 : <span>{{ inputItem }}</span>
        </div>
{% endraw %}
```

### React
```diff
{% raw %}
        const todos = {
          high : ['掃除', '洗濯', '炊事'],
          normal: ['買い物', '草刈り', 'アイロン'],
          low: ['窓拭き', '振り込み', '家計簿管理']
        };
+       const [inputItem, setInputItem] = React.useState('');
{% endraw %}
```

```diff
{% raw %}
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
{% endraw %}
```

[STEP4へ](step4.md)  
[STEP6へ](step6.md)  
