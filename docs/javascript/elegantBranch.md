---
sidebar_position: 8
---

优雅永不过时,浪漫至死不渝。 

向简洁、优雅、健壮、可维护性强的程序，冲冲冲

- 逻辑或运算符 || 俗称短路运算符

用法：优化 if
```js
let result
if (a) {
    result = a
} else if (b) {
    result = b
} else {
    result = c
}

let result = a || b || c
```

- 三元运算符：条件判断、条件赋值、递归；替换判断高度较低的条件语句

```js
let sum = 0;
function increment (n) {
  sum += n
  if (n === 1) 
    return sum
  else 
    return increment(n - 1)
}

let sum = 0;
function increment (n) {
  sum += n
  return n >= 2 ? increment(n - 1) : sum
}
```

- 对象 || 数组配置 条件复杂的多重判断

```js
// 条件语句
let dayNum = 1
let getWeekDay = (dayNum) => {
    if (dayNum === 7) {
        return "星期日"
    } else if (dayNum === 1) {
        return "星期一"
    } else if (dayNum === 2) {
        return "星期二"
    } else if (dayNum === 3) {
        return "星期三"
    } else if (dayNum === 4) {
        return "星期四"
    } else if (dayNum === 5) {
        return "星期五"
    } else if (dayNum === 6) {
        return "星期六"
    } else {
        return "不是合法的值"
    }
}

// 选择语句
let result;
switch (dayNum) {
    case 0: 
        result = "星期日"
        break
    case 1: 
        result = "星期一"
        break
    case 2: 
        result = "星期二"
        break
    case 3: 
        result = "星期三"
        break
    case 4: 
        result = "星期四"
        break
    case 5: 
        result = "星期五"
        break
    case 6: 
        result = "星期六"
        break
    default:
        result = "不是合法的值"
        break
}
return result;

// 数组方式实现
let days = ["星期一", "星期二", "星期三", "星期四", "星期五", "星期六", "星期日"]
let result = days[dayNum] ?? "不是合法的值"

// 对象方式实现
let days = {
    0: "星期日",
    1: "星期一",
    2: "星期二",
    3: "星期三",
    4: "星期四",
    5: "星期五",
    6: "星期六",
}
let result = days[dayNum] ?? "不是合法的值"

// 新增Map实现
let days = new Map([
    [0, "星期日"],
    [1, "星期一"],
    [2, "星期二"],
    [3, "星期三"],
    [4, "星期四"],
    [5, "星期五"],
    [6, "星期六"]
])
let result = days.get(dayNum) || "不是合法的值"
```

- 复杂条件分支优化
```js
let days = {
   0: () => "星期日",
   1: () => "星期一",
   2: () => "星期二",
   3: () => "星期三",
   4: () => "星期四",
   5: () => "星期五",
   6: () => "星期六",
}
let result = days[dayNum] ? days[dayNum]() : "不是合法的值"
// 原始代码
let score = 98;
let getResult = (score) => {
    if (score <= 100 && score >= 90) {
        return "优秀"
    } else if (score < 90 && score >= 80) {
        return "良好"
    } else if (score < 80 && score >= 70) {
        return "中等"
    } else if (score < 70 && score >= 60) {
        return "及格"
    } else if (score < 60 && score >= 0) {
        return "不及格"
    } else {
        return "非法字符"
    }
}

// 二维数组优化
let scores = [
    [(score) => score <= 100 && score >= 90, () => "优秀"],
    [(score) => score <= 90 && score >= 80, () => "良好"],
    [(score) => score <= 80 && score >= 70, () => "中等"],
    [(score) => score <= 70 && score >= 60, () => "及格"],
    [(score) => score <= 60 && score >= 0, () => "不及格"]
]
let getResult = (score) => {
    // 判断符合条件的子数组
    let obj = scores.find(v => v[0](score))
    // 子数组存在，则运行数组中第二个函数元素，否则返回其它信息
    let result = obj ? obj[1]() : "非法字符"
    return result;
}
```