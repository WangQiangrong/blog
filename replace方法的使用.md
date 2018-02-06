#### 方法名：str.replace(regexp|substr, newSubStr|function)

#### 作用：返回一个由替换值替换一些或所有匹配的模式后的新字符串

>参数详解：
```
第一个参数可以为正则对象或者字符串字面量
第二个参数可以为一个新字符串也可以为一个函数，该函数返回替换项
```

##### 1、第二个参数为字符串的情况：
>###### a、常见情况：
```javascript
var str='abcdefg';
var reg=/cd/g;
str=str.replace(reg,'');
console.log(str);   //abefg
```
>###### b、使用变量名替换特定部位：(注意要替换特定部位的话，在正则对象中要是用括号包裹)

+ 变量为$$的情况：插入用$符号替换正则对象中括号包裹的字符所匹配到的内容
```javascript
var str='abcdefg';
var reg=/(c)d/g;
str=str.replace(reg,'$$d');//这里括号包裹的c被$替换了，然后再替换整个匹配到的cde
console.log(str);//ab$defg
```
+ 变量为$n的情况：插入匹配第n个括号中的字符
```javascript
var str='abcdefg';
var reg=/(c)d(e)/g;
str=str.replace(reg,'$2d$1');   //这里把匹配的第一个内容合第二个内容替换位置（$1匹配第一个位置，$2匹配第二个位置），然后再替换整个匹配到的cde
console.log(str);   //abedcfg
```
+ 变量为$&的情况：插入被正则对象匹配到的字符
```javascript
var str='abcdefg';
var reg=/(c)d(e)/g;
str=str.replace(reg,'$&d$1$2');//这里在匹配到的d前面插入整个匹配到的字符cde，在d后面插入匹配到的第一个和第二个括号中的内容，然后再替换整个匹配到的cde
console.log(str);//abcdedcefg
```
+ 变量为$`的情况：插入当前匹配的子串左边的内容
```javascript
var str='abcdefg';
var reg=/(c)d(e)/g;
str=str.replace(reg,'d$`');//这里是把匹配到的字符串cde左边内容ab插入到d后面，然后再替换整个匹配到的cde
console.log(str);//abdabfg
```
+ 变量为$'的情况：插入当前匹配的子串右边的内容
```javascript
var str='abcdefg';
var reg=/(c)d(e)/g;
str=str.replace(reg,"d$'");//这里是把匹配到的字符串cde右边内容fg插入到d后面，然后再替换整个匹配到的cde（注意这里的单引号要用双引号包裹）
console.log(str);//abdfgfg
```

##### 1、第二个参数为函数的情况：
> 替换函数参数详解

变量名|代表的值
-------|----------
match|匹配的子串。（对应于上述的**$&**。）
p1,p2, ...|类似于上述$1，$2……
offset |匹配到的子字符串在原字符串中的偏移量。（比如，如果原字符串是“abcd”，匹配到的子字符串是“bc”，那么这个参数将是1）
string|被匹配的原字符串。

```javascript
var str='xiaoming';
var reg=/(a)o(m)/g;
function replacer(match, p1, p2, offset, string) {
    console.log(match, p1, p2, offset, string);//aom a m 2 xiaoming
  return [p1, p2].join(' - ');
}
var newString = str.replace(reg, replacer);
console.log(newString);  //xia - ming
```