# VUE 
## 初识VUE

~~~vue
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>初识vue</title>
    <!-- 引入Vue -->
    <script src="../js/vue.js"></script>
</head>
<body>
    <!-- 准备好一个容器 -->
    <div id="root">
        <h1>Hello！{{name}}!</h1>
    </div>

    <script>
        Vue.config.productionTip = false // 阻止vue在启动时生成生产提示
        new Vue({
            el:'#root', //el用于指定当前Vue实例为哪个容器服务，值通常为css选择器字符串
            data:{ //data用于存储数据，数据共el所指定的容器去使用
                name:'JOJO'
            }
        })
    </script>
</body>
</html>

```
注意：
- 
- 想让Vue工作，就必须创建一个Vue实例，且要传入一个配置对象
- root容器里的代码依然符合html规范，只不过混入了一些特殊的Vue语法
- root容器里的代码被称为Vue模板
- Vue实例与容器是一一对应的
- 真实开发中只有一个Vue实例，并且会配合着组件一起使用
- {{xxx}}中的xxx要写js表达式，且xxx可以自动读取到data中的所有属性
一旦data中的数据发生变化，那么模板中用到该数据的地方也会自动更新
~~~

## 模板语法
~~~vue
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>vue模板语法</title>
    <script src="../js/vue.js"></script>
</head>
<body>
    <div id="root">
        <h1>插值语法</h1>
        <h3>你好，{{name}}!</h3>
        <hr>
        <h1>指令语法</h1>
        <a v-bind:href="url">快去看新番！</a><br>
        <a :href="url">快去看新番！</a>
    </div>

    <script>
        Vue.config.productionTip = false 
        new Vue({
            el:'#root', 
            data:{ 
                name:'JOJO',
                url:'https://www.bilibili.com/'
            }
        })
    </script>
</body>
</html>
```
~~~
## 数据绑定
~~~vue
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>数据绑定</title>
    <script src="../js/vue.js"></script>
</head>
<body>
    <div id="root">
        单向数据绑定：<input type="text" v-bind:value="name"><br>
        双向数据绑定：<input type="text" v-model:value="name">
    </div>

    <script>
        Vue.config.productionTip = false 
        new Vue({
            el:'#root', 
            data:{
                name:'JOJO'
            }
        })
    </script>
</body>
</html>
```
~~~
***总结*：**

```vue
Vue中有2种数据绑定的方式：

单向绑定（v-bind）：数据只能从data流向页面
双向绑定（v-model）：数据不仅能从data流向页面，还可以从页面流向data
备注：

双向绑定一般都应用在表单类元素上（如：<input>、<select>、<textarea>等）
v-model:value可以简写为v-model，因为v-model默认收集的就是value值
```

## el与data的两种写法
- 函数式写法
- 对象式写法

    ```vue
    
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>el与data的两种写法</title>
        <script src="../js/vue.js"></script>
    </head>
    <body>
        <div id="root">
            <h1>Hello,{{name}}!</h1>
        </div>
    
        <script>
            Vue.config.productionTip = false 
            //el的两种写法：
            // const vm = new Vue({
            //     // el:'#root', //第一种写法
            //     data:{
            //         name:'JOJO'
            //     }
            // })
            // vm.$mount('#root')//第二种写法
    
            //data的两种写法：
            new Vue({
                el:'#root', 
                //data的第一种写法：对象式
                // data:{
                //     name:'JOJO'
                // }
                //data的第二种写法：函数式
                data(){
                    return{
                        name:'JOJO'
                    }
                }
            })
        </script>
    </body>
    </html>
    
    ```
## MVVC模型

    MVVM模型：
    M：模型（Model），data中的数据
    V：视图（View），模板代码
    VM：视图模型（ViewModel），Vue实例

## 事件处理
```vue
        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta http-equiv="X-UA-Compatible" content="IE=edge">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>事件的基本用法</title>
            <script src="../js/vue.js"></script>
        </head>
        <body>
            <div id="root">
                <h2>hello,{{name}}</h2>
                <button v-on:click="showInfo1">点我提示信息1</button>
                <button @click="showInfo2($event,66)">点我提示信息2</button>
            </div>
        
            <script>
                Vue.config.productionTip = false 
                new Vue({
                    el:'#root', 
                    data:{
                        name:'JOJO'
                    },
                    methods:{
                        showInfo1(event){
                            console.log(event)
                        },
                        showInfo2(evnet,num){
                            console.log(event,num)
                        }
                    }
                })
            </script>
        </body>
    </html>
```
总结：

> 
>     使用v-on:xxx或@xxx绑定事件，其中xxx是事件名
>     
>     事件的回调需要配置在methods对象中，最终会在vm上
>     
>     methods中配置的函数，==不要用箭头函数！==否则this就不是vm了
>     
>     methods中配置的函数，都是被Vue所管理的函数，this的指向是vm或组件实例对象
>     
>     @click="demo和@click="demo($event)"效果一致，但后者可以传参
> 

## 事件装饰符

> Vue中的事件修饰符：
> 
> prevent：阻止默认事件（常用）
> stop：阻止事件冒泡（常用）
> once：事件只触发一次（常用）
> capture：使用事件的捕获模式
> self：只有event.target是当前操作的元素时才触发事件
> passive：事件的默认行为立即执行，无需等待事件回调执行完毕


```vue
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>事件修饰符</title>
		<script type="text/javascript" src="../js/vue.js"></script>
		<style>
			*{
				margin-top: 20px;
			}
			.demo1{
				height: 50px;
				background-color: skyblue;
			}
			.box1{
				padding: 5px;
				background-color: skyblue;
			}
			.box2{
				padding: 5px;
				background-color: orange;
			}
			.list{
				width: 200px;
				height: 200px;
				background-color: peru;
				overflow: auto;
			}
			li{
				height: 100px;
			}
		</style>
	</head>
	<body>
		<div id="root">
			<h2>欢迎来到{{name}}学习</h2>
			<!-- 阻止默认事件 -->
			<a href="http://www.atguigu.com" @click.prevent="showInfo">点我提示信息</a>

			<!-- 阻止事件冒泡 -->
			<div class="demo1" @click="showInfo">
				<button @click.stop="showInfo">点我提示信息</button>
			</div>

			<!-- 事件只触发一次 -->
			<button @click.once="showInfo">点我提示信息</button>

			<!-- 使用事件的捕获模式 -->
			<div class="box1" @click.capture="showMsg(1)">
				div1
				<div class="box2" @click="showMsg(2)">
					div2
				</div>
			</div>

			<!-- 只有event.target是当前操作的元素时才触发事件 -->
			<div class="demo1" @click.self="showInfo">
				<button @click="showInfo">点我提示信息</button>
			</div>

			<!-- 事件的默认行为立即执行，无需等待事件回调执行完毕 -->
			<ul @wheel.passive="demo" class="list">
				<li>1</li>
				<li>2</li>
				<li>3</li>
				<li>4</li>
			</ul>

		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false

		new Vue({
			el:'#root',
			data:{
				name:'尚硅谷'
			},
			methods:{
				showInfo(e){
					alert('同学你好！')
				},
				showMsg(msg){
					console.log(msg)
				},
				demo(){
					for (let i = 0; i < 100000; i++) {
						console.log('#')
					}
					console.log('累坏了')
				}
			}
		})
	</script>
</html>

```

## 键盘事件
```Vue
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>键盘事件</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<div id="root">
			<h2>欢迎来到{{name}}学习</h2>
			<input type="text" placeholder="按下回车提示输入" @keydown.enter="showInfo">
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false

		new Vue({
			el:'#root',
			data:{
				name:'尚硅谷'
			},
			methods: {
				showInfo(e){
					console.log(e.target.value)
				}
			},
		})
	</script>
</html>
```

> Vue中常用的按键别名：
> 
> 回车：enter
> 删除：delete (捕获“删除”和“退格”键)
> 退出：esc
> 空格：space
> 换行：tab (特殊，必须配合keydown去使用)
> 上：up
> 下：down
> 左：left
> 右：right
> 注意：
系统修饰键（用法特殊）：ctrl、alt、shift、meta
> 
> 配合keyup使用：按下修饰键的同时，再按下其他键，随后释放其他键，事件才被触发
> 配合keydown使用：正常触发事件
> 可以使用keyCode去指定具体的按键，比如：@keydown.13="showInfo"，但不推荐这样使用
> 
> Vue.config.keyCodes.自定义键名 = 键码，可以自定义按键别名

## 计算属性
```vue

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>计算属性</title>
    <script src="../js/vue.js"></script>
</head>
<body>
    <div id="root">
        姓：<input type="text" v-model="firstName"><br><br>
        名：<input type="text" v-model="lastName"><br><br>
        姓名：<span>{{fullName}}</span>
    </div>

    <script>
        Vue.config.productionTip = false 

        new Vue({
            el:'#root', 
            data:{ 
                firstName:'张',
                lastName:'三'
            },
            computed:{
                fullName:{
                    get(){
                        return this.firstName + '-' + this.lastName
                    },
                    set(value){
						const arr = value.split('-')
						this.firstName = arr[0]
						this.lastName = arr[1]
                    }
                }
            }
        })
    </script>
</body>
</html>

```
## 监视属性

### 基本监视
```vue

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>监视属性</title>
    <script src="../js/vue.js"></script>
</head>
<body>
    <div id="root">
        <h2>今天天气好{{info}}!</h2>
        <button @click="changeWeather">点击切换天气</button>
    </div>

    <script>
        Vue.config.productionTip = false 

        new Vue({
            el:'#root', 
            data:{ 
                isHot:true,
            },
            computed:{
                info(){
                    return this.isHot ? '炎热' : '凉爽' 
                }
            },
            methods:{
				changeWeather(){
					this.isHot = !this.isHot
				}
			},
            watch:{
                isHot:{
                    immediate:true, //初始化时让handler调用一下
                    //handler什么时候调用？当isHot发生改变时
                    handler(newValue,oldValue){
						console.log('isHot被修改了',newValue,oldValue)
					}
                }
            }
        })
    </script>
</body>
</html>

```
### 深度监视

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>深度监视</title>
    <script src="../js/vue.js"></script>
</head>
<body>
    <div id="root">
        <h3>a的值是:{{numbers.a}}</h3>
		<button @click="numbers.a++">点我让a+1</button>
		<h3>b的值是:{{numbers.b}}</h3>
		<button @click="numbers.b++">点我让b+1</button>
    </div>

    <script>
        Vue.config.productionTip = false 

        new Vue({
            el:'#root', 
            data:{ 
                isHot:true,
                numbers:{
                    a:1,
                    b:1,
                }
            },
            watch:{
                //监视多级结构中所有属性的变化
                numbers:{
                    deep:true,
					handler(){
						console.log('numbers改变了')
					}
                }
                //监视多级结构中某个属性的变化
				/* 'numbers.a':{
					handler(){
						console.log('a被改变了')
					}
				} */
            }
        })
    </script>
</body>
</html>
```
## 列表渲染
```vue

<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>基本列表</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<div id="root">
			<h2>人员列表（遍历数组）</h2>
			<ul>
				<li v-for="(p,index) in persons" :key="index">
					{{p.name}}-{{p.age}}
				</li>
			</ul>

			<h2>汽车信息（遍历对象）</h2>
			<ul>
				<li v-for="(value,k) in car" :key="k">
					{{k}}-{{value}}
				</li>
			</ul>

			<h2>遍历字符串</h2>
			<ul>
				<li v-for="(char,index) in str" :key="index">
					{{char}}-{{index}}
				</li>
			</ul>
			
			<h2>遍历指定次数</h2>
			<ul>
				<li v-for="(number,index) in 5" :key="index">
					{{index}}-{{number}}
				</li>
			</ul>
		</div>

		<script type="text/javascript">
			Vue.config.productionTip = false
			
			new Vue({
				el:'#root',
				data:{
					persons:[
						{id:'001',name:'张三',age:18},
						{id:'002',name:'李四',age:19},
						{id:'003',name:'王五',age:20}
					],
					car:{
						name:'奥迪A8',
						price:'70万',
						color:'黑色'
					},
					str:'hello'
				}
			})
		</script>
    </body>
</html>

```

## 表单提交
```vue
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>收集表单数据</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<div id="root">
			<form @submit.prevent="demo">
				账号：<input type="text" v-model.trim="userInfo.account"> <br/><br/>
				密码：<input type="password" v-model="userInfo.password"> <br/><br/>
				年龄：<input type="number" v-model.number="userInfo.age"> <br/><br/>
				性别：
				男<input type="radio" name="sex" v-model="userInfo.sex" value="male">
				女<input type="radio" name="sex" v-model="userInfo.sex" value="female"> <br/><br/>
				爱好：
				学习<input type="checkbox" v-model="userInfo.hobby" value="study">
				打游戏<input type="checkbox" v-model="userInfo.hobby" value="game">
				吃饭<input type="checkbox" v-model="userInfo.hobby" value="eat">
				<br/><br/>
				所属校区：
				<select v-model="userInfo.city">
					<option value="">请选择校区</option>
					<option value="beijing">北京</option>
					<option value="shanghai">上海</option>
					<option value="shenzhen">深圳</option>
					<option value="wuhan">武汉</option>
				</select>
				<br/><br/>
				其他信息：
				<textarea v-model.lazy="userInfo.other"></textarea> <br/><br/>
				<input type="checkbox" v-model="userInfo.agree">阅读并接受<a href="http://www.atguigu.com">《用户协议》</a>
				<button>提交</button>
			</form>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false

		new Vue({
			el:'#root',
			data:{
				userInfo:{
					account:'',
					password:'',
					age:0,
					sex:'female',
					hobby:[],
					city:'beijing',
					other:'',
					agree:''
				}
			},
			methods: {
				demo(){
					console.log(JSON.stringify(this.userInfo))
				}
			}
		})
	</script>
</html>

```
## 过滤器

```vue

<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>过滤器</title>
		<script type="text/javascript" src="../js/vue.js"></script>
		<script src="https://cdn.bootcdn.net/ajax/libs/dayjs/1.10.6/dayjs.min.js"></script>
	</head>
	<body>
		<div id="root">
			<h2>时间</h2>
            <h3>当前时间戳：{{time}}</h3>
            <h3>转换后时间：{{time | timeFormater()}}</h3>
			<h3>转换后时间：{{time | timeFormater('YYYY-MM-DD HH:mm:ss')}}</h3>
			<h3>截取年月日：{{time | timeFormater() | mySlice}}</h3>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false
		//全局过滤器
		Vue.filter('mySlice',function(value){
			return value.slice(0,11)
		})
		new Vue({
            el:'#root',
            data:{
                time:1626750147900,
            },
			//局部过滤器
            filters:{
                timeFormater(value, str="YYYY年MM月DD日 HH:mm:ss"){
                    return dayjs(value).format(str)
                }
            }
        })
	</script>
</html>

```

## 生命周期
```vue
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>自定义指令</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
    <!-- 
		需求1：定义一个v-big指令，和v-text功能类似，但会把绑定的数值放大10倍。
		需求2：定义一个v-fbind指令，和v-bind功能类似，但可以让其所绑定的input元素默认获取焦点。
	-->
	<body>
		<div id="root">
			<h2>当前的n值是：<span v-text="n"></span> </h2>
			<h2>放大10倍后的n值是：<span v-big="n"></span> </h2>
			<button @click="n++">点我n+1</button>
			<hr/>
			<input type="text" v-fbind:value="n">
		</div>
	</body>
	
	<script type="text/javascript">
		Vue.config.productionTip = false

		new Vue({
			el:'#root',
			data:{
				n:1
			},
			directives:{
                //big函数何时会被调用？1.指令与元素成功绑定时（一上来） 2.指令所在的模板被重新解析时
				big(element,binding){
					console.log('big',this) //注意此处的this是window
					element.innerText = binding.value * 10
				},
				fbind:{
					//指令与元素成功绑定时（一上来）
					bind(element,binding){
						element.value = binding.value
					},
					//指令所在元素被插入页面时
					inserted(element,binding){
						element.focus()
					},
					//指令所在的模板被重新解析时
					update(element,binding){
						element.value = binding.value
					}
				}
			}
		})
	</script>
</html>

```