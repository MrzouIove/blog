# vue引入Cookie（js-cookie插件）的使用方法

## 1.安装

```shell
npm install js-cookie --save
```

## 2.引用

```shell
import Cookies from 'js-cookie'
```

## 3.一般使用

### （1）存到Cookie去

```shell
// Create a cookie, valid across the entire site:
Cookies.set('name', 'value');

// Create a cookie that expires 7 days from now, valid across the entire site:
Cookies.set('name', 'value', { expires: 7 });

// Create an expiring cookie, valid to the path of the current page:
Cookies.set('name', 'value', { expires: 7, path: '' });
```

### （2）在Cookie中取出

```shell
// Read cookie:
Cookies.get('name'); // => 'value'
Cookies.get('nothing'); // => undefined

// Read all visible cookies:
Cookies.get(); // => { name: 'value' }
```

### 	(3)删除

```shell
// Delete cookie:
Cookies.remove('name');

// Delete a cookie valid to the path of the current page:
Cookies.set('name', 'value', { path: '' });
Cookies.remove('name'); // fail!
Cookies.remove('name', { path: '' }); // removed!
```

### 4.特殊使用（在Cookie中存对象）

跟一般使用不同的是，从Cookie中取出的时候，要从字符串转换成json格式：

```shell
const user = {
  name: 'lia',
  age: 18
}
Cookies.set('user', user)
const liaUser = JSON.parse(Cookies.get('user'))
```


