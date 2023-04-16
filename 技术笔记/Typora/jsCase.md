##### 1.获取文件后缀名

```js
/**
 * 1.获取文件后缀名
 */
export function getExt(filename){
    if(typeof filename == 'string'){
        return filename.split('.').pop().toLowerCase();
    }else{
        throw new Error('filename must be a string type');
    }
}
//使用方式
getExt("1.mp4")//mp4
```

##### 2.复制内容到剪贴板

```js
/**
 * 2.复制内容到剪贴板
 *  原理：1.创建一个textarea元素并调用select()方法选中
 *        2.document.execCommand('copy')方法拷贝当前选中内容到剪贴板
 */
export function copyToBoard(value){
    const element = document.createElement('textarea');
    document.body.appendChild(element);
    element.value = value;
    element.select();
    if(document.execCommand('copy')){
        document.execCommand('copy');
        document.body.removeChild(element);
        return true;
    }
    document.body.removeChild(element);
    return false;
}
//使用方式，如果复制成功返回true
copyToBoard('lalalal');
```

##### 3.休眠xx毫秒(ms)

```js
/**
 * 3.休眠xx毫秒(ms)
 */
export function sleep(ms){
    return new Promise(resolve => setTimeout(resolve,ms))
}
//使用方式
const fetchData = async() => {
    await sleep(1000);
}
```

##### 4.生成随机字符串

```js
/**
 * 4.生成随机字符串
 *  使用场景：前端生成随机ID
 */
export function uuid(length,chars){
    chars = chars || '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVMXYZ';
    length = length || 8;
    var result = '';
    for(var i = length; i > 0; --i){
        result += chars[Math.floor(Math.random() * chars.length)];
    }
    return result;
}
```

##### 5.简单的深拷贝

```js
/**
 * 5.简单的深拷贝
 *  缺陷：只拷贝对象、数组以及对象数组
 */
export function deepCopy(obj){
    if(typeof obj != 'object'){
        return obj;
    }
    if(obj == null){
        return obj;
    }
    return JSON.parse(JSON.stringify(obj));
}
//使用方式
const person = {name:'xiaoming',child:{name:'Jack'}};
const person2 = deepCopy(person);
```

##### 6.数组去重

```js
/**
 * 6.数组去重
 *  原理：利用Set中不能出现重复元素的特性
 */
export function uniqueArray(arr){
    if(!Array.isArray(arr)){
        throw new Error('The first parameter must be an array');
    }
    if(arr.length == 1){
        return arr;
    }
    return [...new Set(arr)];
}
//使用方式
uniqueArray([1,1,1]);//[1]
```

##### 7.对象转化为FormData对象

```js
/**
 * 7.对象转化为FormData对象
 *  使用场景：上传文件时我们要新建一个FormData对象，然后有多少个参数就append多少次，使用该函数可以简化逻辑
 */
export function getFormData(object){
    const formData = new FormData();
    Object.keys(object).forEach(key => {
        const value = object[key]
        if(Array.isArray(value)){
            value.forEach((subValue,i) => 
                formData.append(key + `[${i}]`,subValue)
            );
        }else{
            formData.append(key,object[key]);
        }
    });
    return formData;
}
//使用方式
let req = {
    file:xxx,
    userId:1,
    phone:'15112345678',
    //...
}
fetch(getFormData(req));
```

##### 8.保留小数点后几位，默认两位

```js
/**
 * 8.保留小数点后几位，默认两位
 */
export function cutNumber(number, no = 2){
    if(typeof number != 'number'){
        number = Number(number);
    }
    return Number(number.toFixed(no));
}
```

##### 9.JS底层保存标识符

实际上是采用Unicode编码，所以理论上讲，所有的utf-8中含有的内容都可以作为标识符。比如：

```js
var 张三 = "abc";
console.log(张三);
```

##### 10.CSS的权重

它指的是样式的优先级，有两条或多条样式作用于一个元素,权重高的那条样式对元素起作用,如果权重相同,那么后写的样式会覆盖前面写的样式。

—级、!important加在样式属性后,权重值为10000(慎用)
二级、内联样式(style=" ")权重值为1000

三级、ID选择器(#con)权重值为100
四级、类，伪类和属性选择器(.con, :hover,input[type='text'])权重值为10
五级、标签选择器和伪元素选择器(div 、 :before)权重值为1
六级:通用选择器(*)、子选择器(>)、相邻选择器(+)、同胞选择器(~)权重值为0

