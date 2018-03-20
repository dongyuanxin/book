### ES5下的`var`
> 由于`js`变量提升法则，`var`不仅仅在它的代码作用域有用！可以看以下demo。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <p>黄义军</p><br/>
    <p>黄鑫</p><br/>
    <script>
        let ps = document.getElementsByTagName('p')
        for(var i=0;i<ps.length;++i ) {
            ps[i].onclick = e => console.log(i)
        }
    </script>
</body>
</html>
```

分别点击两段文字，控制台输出的都是`2`。按道理应该是`1`或`2`。这是因为：`JavaScript`引擎内部会记住上一轮循环的值，初始化本轮的变量i时，就在上一轮循环的基础上进行计算。**也就是说，`var`声明的变量的作用域并不是自身作用域。**

当然了，可以利用闭包来规避这种问题，请看：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <p>黄义军</p><br/>
    <p>黄鑫</p><br/>
    <script>
        let ps = document.getElementsByTagName('p')
        let _ = i => event => console.log(i) // 返回一个函数，并且函数内部取到了正确的 i 值
        for(var i=0;i<ps.length;++i ) {
            ps[i].onclick = _(i)
            // 循环的每一步都将当前i的值复制一份
        }
    </script>
</body>
</html>
```

### ES6下的`let`
> 有了`let`，之前关于`var`的问题都解决了。**强烈建议：将使用`var`的地方替换为`let`！！！**。

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <p>黄义军</p><br/>
    <p>黄鑫</p><br/>
    <script>
        let ps = document.getElementsByTagName('p')
        for(let i=0;i<ps.length;++i ) { // 利用let声明
            ps[i].onclick = event => console.log(i) 
        }
    </script>
</body>
</html>
```

### 更多思考
- `var`并不是一无是处！之前看过一篇文章：`Node`的`V8`引擎在优化的时候，是不能优化`let`。文中还给出了测试代码。以后如果有时间再来填坑。

- 关于结合`of`的问题（可以测试以下代码）

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <p>黄义军</p><br/>
    <p>黄鑫</p><br/>
    <script>
        // 错误写法：( 注意 i 的作用域)
        let ps = document.getElementsByTagName('p')
		let i = 0
		for(let p of ps ) { // 利用let声明
            p.onclick = event => console.log(i) 
			i+=1
        }
        // 正确写法：
        // let ps = document.getElementsByTagName('p')
		// let helper = (i) => e => console.log(i)
		// let i = 0
		// for(let p of ps ) { // 利用let声明
        //     p.onclick = helper(i)
		// 	i+=1
        // }
    </script>
</body>
</html>
```
