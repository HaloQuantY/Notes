## JavaScript基础算法（freeCodeCamp）

### 字符串和数组相互转换

#### 字符串→数组

- split（）
- Array.from（）

#### 数组→字符串

- join（）

```javascript
// Reverse the provided string.
function reverseString(str) {
  const arr = str.split("");
  str = arr.reverse().join("");
  return str;
}

reverseString("hello");
```



### 字符串子串方法

- slice（）

  第一个参数是起始位置，第二个参数结束位置，省略第二个参数则默认截取到底

  当参数为负数，将两参数转为从后向前位置

- substring（）

  第一个参数是起始位置，第二个参数结束位置，省略第二个参数则默认截取到底

  参数为负数，将第一个参数转为从后向前位置，第二参数转为0

- substr（）

  第一个参数是起始位置，第二个参数是要截取字符数量，省略第二参数默认截取到底

  参数为负数，将两参数都转为0

```javascript
// Check if a string (first argument, str) ends with the given target string (second argument, target).
function confirmEnding(str, target) {
  const substr = str.slice(-target.length);
  if (substr === target) {
    return true;
  } else {
    return false;
  }
}

confirmEnding("Bastian", "n");
```



### 关于Boolean类型

#### typeof和instanceof

由于Boolean类型是基本数据类型而非引用类型，所以对Boolean类型使用instanceof Boolean会获得false。

所以判断变量是否为Boolean类型需要使用`if (typeof true === "boolean")`

#### Boolean构造函数

- 使用`new Boolean()`方法创建的都是Boolean对象，即使参数为false，最后创建得到的都是引用类型值，参加逻辑判断时为true。
- 要将一个数据转换为Boolean类型可以使用Boolean转型函数`Boolean()`或者利用一些语句的自动转换机制（例如if语句圆括号中会自动转换），尽量不使用new来调用Boolean构造函数



### slice和splice方法

#### slice方法

- 用于对一个数组切片，并将切片元素返回到新数组
- 接收两个参数，第一个参数为起始下标，第二个参数为结束下标，省略第二个参数则会从起始下标一直截取到结束所有项
- slice方法不会影响原数组，而是返回切下的数组

#### splice方法

- 有删除，插入和替换三种用法，其中对数组中部插入项是主要用法
- 接收2个以上参数，第一个参数是起始下标，第二个参数是删除元素的个数，第三及之后的参数为要插入的元素
- splice方法会改变原数组，且总返回被删除元素构成数组，如果没有删除则返回空数组



### 数组排序方法

#### sort和reverse方法

- sort（）

  默认情况下升序排列，将数组中元素全部转换为字符串进行比较。

  然而默认将元素转换为字符串后数字排序不准确，需要传入参数控制排序。

  传入的参数为一个回调函数，回调函数有两个参数value1和value2，其中value1代表数组中前一个元素，value2代表数组中后一个元素

  回调函数返回值为正数，两数交换位置，返回值为负数和0则不会交换

- reverse（）

  reverse方法直接将数组中元素顺序倒序排列

两个排序方法都会改变原数组，是对原数组进行直接修改。