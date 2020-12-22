---
title: FE Algorithm - 阶乘
---

## 阶乘

### 递归

``` tsx
/*
* 阶乘
* @param {number} n 需要求的阶乘
* @return {number} 阶乘值
*/
function factorialize(n){
	if(typeof n !== 'number') throw new Error('参数必须为整整')
	if(n === 1) return 1;
	// 建议不要使用 arguments.callee，目前已经废弃了。
	return n * factorialize(n - 1);
}
```

> *Note：上面代码是一个阶乘函数，计算n的阶乘，最多需要保存n个调用记录，复杂度 O(n) 。*
> *递归非常耗费内存，因为需要同时保存成千上百个调用帧，很容易发生“栈溢出”错误（stack overflow）。*

### ES6尾调用优化

``` tsx
function factorialize(n, result = 1){
  if(typeof n !== 'number' || typeof result !== 'number') throw new Error('参数必须为整整')
  if (n === 1) return result;
  return factorialize(n - 1, n * result);
}
```

### 数组存储实现

> 算法思路
> 可以使用数组来存储大数据结果的每位数，如result[0]存储个位数，result[1]存储十位数，以此类推。计算每位数时需要加上上一个位数得出的进位，最后再将数组反转并拼接，就可以得出大数据结果。

> 算法分析
> 以5! = 1 * 2 * 3 * 4 * 5为例：

 ![](https://user-gold-cdn.xitu.io/2019/9/21/16d52eed058e670e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1) 


``` tsx
function factorial_array(n){
    let result = [1],   //存储结果
        digit = 1,      //位数，从第1位开始
        count ,         //每次计算的结果
        num ,           //阶乘的计算到的第几个
        i ,             //result中每一项
        carry;          //每次得数的进位

    for(num = 2 ; num <= n ; num++){
        for(i = 1 , carry = 0 ; i <= digit ; i++){
            count = result[i - 1] * num + carry;        //每一项计算结果
            result[i - 1] = count % 10;                 //将一个数的每一位数利用数组进行储存
            carry = (count - result[i - 1]) / 10        //记录进位
        }

        while (carry) { //如果还有进位，继续存储
            result[digit] = carry % 10;
            carry = (carry - result[digit]) / 10;
            digit++;
        }
    }
    return result.reverse().join("");
}
```

********
参考链接: 
- [前端基础算法](https://juejin.cn/post/6844903590595674125)
- [面试题之Javascript实现1万的阶乘](https://juejin.cn/post/6844903949737164814)
