---
title: 高阶函数
date: 2021-01-12 16:17:30
updated: 2021-01-12 16:18:30
tags: 技术
categories: 编程
---

### 高阶函数介绍
用于过程抽象，提取相同逻辑便于复用。概念与函数的装饰器类似。

#### 概念

高阶函数，即返回一个函数的函数。它可以在原基础上对函数做出若干改进，增加一些新的功能或者增强。

#### Once函数
Once函数使传入的函数只会运行一次，用于单例模式初始化，删除元素后阻止响应。
```javascript
/**
 * Only allow the function run once
 * @param {[Function]} fun The function need to be changed 
 */
function once(fun) {
  return function(...args) {
    if (fun) {
      const ret = fun.apply(this, args);
      fun = null;
      return ret;
    }
  }
}
```

### throttle函数
throttle函数，即节流函数。在特定时间里只运行一次，丢弃之后的执行。
```javascript
/**
 * Only run once in the specified time range.
 * @param {[Function]} fun The function need to be changed 
 * @param {[Number]} ms The time need to be throttle
 */
function throttle(fun, ms) {
  let throttleTimer = null;
  return function(...args) {
    if (!throttleTimer) {
      throttleTimer = setTimeout(function() {
        const ret = fun.apply(this, args);
        throttleTimer = null;
        return ret;
      }, ms)
    }
  }
}
```

### debounce函数
防抖函数，在特定时间段内取消重复的运行，只会运行最后一次。
```javascript
/**
 * Only run the last time in the specified time range.
 * @param {[Function]} fun The function need to be changed 
 * @param {[Number]} ms The time need to be debounced
 */
function debounce(fun, ms) {
  let debounceTimer = null;
  return function(...args) {
    if (debounceTimer) {
      clearTimeout(debounceTimer);
    }
    debounceTimer = setTimeout(function() {
      const ret = fun.apply(this, args);
      return ret;
    }, ms)
  }
}
```

### wait函数（工具函数）
setTimeout函数的Promise版重新组织方式
```javascript
/**
 * 
 * @param {number} duration The time need to waiting.
 */
function wait(duration) {
  return new Promise((resolve, reject) => {
    setTimeout(resolve, duration);
  })
}
```