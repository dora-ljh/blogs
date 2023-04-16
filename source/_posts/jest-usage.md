---
title: Jest 的用法
date: 2023-04-16 22:15:17
tags: [Jest, 单元测试]
categories: [技术]
---

在软件开发的过程中，单元测试是必不可少的一部分。Jest 是一个流行的 JavaScript 单元测试框架，它能够帮助我们轻松地进行测试，并提供了许多有用的测试工具和断言。本篇博客将着重介绍 Jest 中的 `toEqual` 和 `toStrictEqual` 的区别，并通过测试随机数的例子来演示 Jest 的用法。

<!-- more -->

### `toEqual`和`toStrictEqual` 的区别


在Jest测试中，`toEqual`和`toStrictEqual`都是用于比较两个对象是否相等的匹配器。它们的主要区别在于对待对象属性值的方式不同。

具体来说，`toEqual`匹配器将比较两个对象的属性值是否相等，而不考虑属性值的类型。这意味着如果两个对象的属性值在逻辑上相等，但类型不同，`toEqual`匹配器仍然会认为它们相等。

相反，`toStrictEqual`匹配器将比较两个对象的属性值和类型是否完全相同。这意味着如果两个对象的属性值虽然逻辑上相等，但是类型不同，`toStrictEqual`匹配器将认为它们不相等。

例如，假设您有以下两个对象：

```javascript
const obj1 = { a: 1 };
const obj2 = { a: '1' };
```

使用`toEqual`匹配器进行比较时，这两个对象将被认为是相等的，因为它们的属性值在逻辑上相等，即`obj1.a === obj2.a`。

```javascript
expect(obj1).toEqual(obj2); // 通过
```

但是，使用`toStrictEqual`匹配器进行比较时，这两个对象将被认为是不相等的，因为它们的属性值类型不同，即`typeof obj1.a !== typeof obj2.a`。

```javascript
expect(obj1).not.toStrictEqual(obj2); // 通过
```

因此，当您需要比较对象的属性值和类型时，您应该使用`toStrictEqual`匹配器。如果您只需要比较对象的属性值是否相等，则可以使用`toEqual`匹配器。

### 测试随机数
在 Jest 中，您可以使用 mock 函数模拟随机数生成器的行为，以便您可以测试随机数的结果。下面是一个示例：

```javascript
// 测试函数
function getRandomNumber() {
  return Math.random() * 100;
}

test('random number test', () => {
  // 创建 mock 函数
  const mockMath = Object.create(global.Math);
  mockMath.random = () => 0.5;
  global.Math = mockMath;

  // 验证随机数生成结果
  expect(getRandomNumber()).toBe(50);

  // 恢复原有 Math 函数
  global.Math = Object.create(Math);
});
```

在上述代码中，我们创建了一个 mock 函数 `mockMath` 来模拟 `Math.random()` 的结果。我们使用 `Object.create()` 创建一个对象来继承全局的 `Math` 对象，并将 `random()` 方法重写为始终返回 0.5。然后我们使用 `global` 对象将 `Math` 对象替换为我们的 mock 对象，以便在测试中使用。我们调用 `getRandomNumber()` 函数并验证它返回的结果是否为预期的结果 50。最后，我们使用 `Object.create()` 恢复全局的 `Math` 对象，以确保我们的测试不会影响其他测试或代码。
