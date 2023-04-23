---
title: Jest 的用法
date: 2023-04-16 22:15:17
tags: [Jest, 单元测试]
categories: [技术]
---

在软件开发的过程中，单元测试是必不可少的一部分。Jest 是一个流行的 JavaScript 单元测试框架，它能够帮助我们轻松地进行测试，并提供了许多有用的测试工具和断言。本篇博客将着重介绍 Jest 的一些用法，并通过一些例子来演示 Jest 的用法。

<!-- more -->

## API用法

### beforeEach
`beforeEach` 是 Jest 提供的一个函数，它会在每个测试用例（`it` 或 `test`）执行前执行一次，用于准备测试用例所需要的数据或环境。例如，假设我们有以下两个测试用例：

```javascript
describe('测试beforeEach', () => {
  it('正数应该返回true', () => {
    expect(myFunction(1)).toBe(true);
    expect(myFunction(2)).toBe(true);
    expect(myFunction(3)).toBe(true);
  });

  it('负数返回false', () => {
    expect(myFunction(-1)).toBe(false);
    expect(myFunction(-2)).toBe(false);
    expect(myFunction(-3)).toBe(false);
  });
});
```

我们可以在每个测试用例里分别调用 `myFunction` 来测试它的正确性，但这样会导致代码重复，而且每个测试用例都需要调用同一个 `myFunction` 函数，效率低下。为了避免这些问题，我们可以使用 `beforeEach` 来准备 `myFunction` 函数：

```javascript
describe('测试beforeEach', () => {
  let myFunction;

  beforeEach(() => {
    myFunction = jest.fn(num => num > 0);
  });

  it('正数应该返回true', () => {
    expect(myFunction(1)).toBe(true);
    expect(myFunction(2)).toBe(true);
    expect(myFunction(3)).toBe(true);
  });

  it('负数返回false', () => {
    expect(myFunction(-1)).toBe(false);
    expect(myFunction(-2)).toBe(false);
    expect(myFunction(-3)).toBe(false);
  });
});
```

在上面的例子中，我们使用了 `beforeEach` 来初始化一个模拟的 `myFunction` 函数，它会根据传入的参数返回一个布尔值，表示参数是否大于 0。这样，在每个测试用例里我们就可以直接使用 `myFunction` 函数了，而不需要每个测试用例里都写一遍相同的代码。

### toContain
在 Jest 中，`toContain` 是一个匹配器（matcher），用于检查某个值是否包含在另一个值中。具体来说，它可以用于以下数据类型：

*   字符串：检查字符串是否包含指定的子字符串。
*   数组：检查数组是否包含指定的元素。
*   Set：检查 Set 是否包含指定的元素。
*   Map：检查 Map 是否包含指定的键。
*   Object：检查对象是否包含指定的属性。

`toContain` 的使用方式如下：

```javascript
test('检查数组是否包含值', () => {
  const arr = [1, 2, 3];
  expect(arr).toContain(2);
  expect(arr).not.toContain(4);
});

test('检查字符串是否包含子字符串', () => {
  const str = 'hello, world!';
  expect(str).toContain('world');
  expect(str).not.toContain('foo');
});

test('检查Set是否包含值', () => {
  const set = new Set([1, 2, 3]);
  expect(set).toContain(2);
  expect(set).not.toContain(4);
});

test('检查Map是否包含键', () => {
  const map = new Map([['foo', 1], ['bar', 2], ['baz', 3]]);
  expect(map).toContain('foo');
  expect(map).not.toContain('qux');
});

test('检查对象是否包含属性', () => {
  const obj = { foo: 1, bar: 2, baz: 3 };
  expect(obj).toHaveProperty('foo');
  expect(obj).not.toHaveProperty('qux');
});
```

在上面的例子中，我们分别使用 `toContain` 来检查数组、字符串、Set、Map 和对象是否包含指定的值或属性。如果匹配成功，则测试通过；否则测试失败。

## 区别

### `it` 和 `test` 的区别

在 Jest 中，`it` 和 `test` 是用于编写测试用例的两个方法，它们没有本质的区别，只是用法上有些微小的差别。

`test` 方法是 Jest 提供的顶层方法，用于创建一个测试用例。它的语法如下：

```javascript
test(description, testFunction, timeout);
```

其中 `description` 是一个字符串，描述这个测试用例的名称；`testFunction` 是一个函数，包含测试代码的实现；`timeout` 可选，指定测试超时时间。

`it` 方法也是 Jest 提供的顶层方法，它与 `test` 方法的作用是完全一样的，只是语法略有不同：


```javascript
it(description, testFunction, timeout);
```

可以看到，`it` 方法的参数与 `test` 方法的参数是一样的，只是方法名不同。在 Jest 中，通常使用 `test` 方法来编写测试用例，因为它更直观、更符合自然语言的习惯。但是如果你习惯使用 `it` 方法，也是完全可以的。

例如，下面的两个测试用例是等价的：

```javascript
test('1 + 1 = 2', () => {
  expect(1 + 1).toBe(2);
});

it('1 + 1 = 2', () => {
  expect(1 + 1).toBe(2);
});
```

无论你使用 `test` 还是 `it` 方法，都能够达到相同的测试效果。

### `toBeGreaterThanOrEqual` 和 `toBeLessThan`还有`toBe`的区别
`toBeGreaterThanOrEqual`和`toBeLessThan`是针对数值的匹配器，分别用于比较实际值是否大于等于或小于指定值，而`toBe`匹配器用于判断实际值是否恰好等于预期值。

例如：

```javascript
expect(5).toBeGreaterThanOrEqual(3); // 通过
expect(5).toBeGreaterThanOrEqual(5); // 通过
expect(5).toBeGreaterThanOrEqual(7); // 不通过

expect(5).toBeLessThan(7); // 通过
expect(5).toBeLessThan(5); // 不通过
expect(5).toBeLessThan(3); // 不通过

expect(5).toBe(5); // 通过
expect(5).toBe(6); // 不通过
```

需要注意的是，`toBe`匹配器使用的是 JavaScript 中的严格相等比较运算符（`===`），而且它不会进行类型转换。


### `toEqual`和`toBe` 的区别
`toEqual`方法比较两个对象的内容，如果它们的属性和属性值都相同，则它们相等。这种比较是递归进行的，也就是说，如果两个对象的属性都是对象，则会比较它们的子属性，直到找到基本类型的属性值为止。这种比较方式称为“深比较”。

相反，`toBe`方法用于比较两个对象的引用是否相同，如果它们引用的是同一个对象，则它们相等。在这种情况下，Jest使用`Object.is`方法进行比较，这种比较方式称为“浅比较”。

例如：
```javascript
it('`toEqual`和`toBe` 的区别', () => {
  const obj = { a: 1, b: 2 };
  const obj1 = obj;
  expect(obj).toBe({ a: 1, b: 2 }); // 不通过
  expect(obj).toEqual({ a: 1, b: 2 }); // 通过
  expect(obj).toBe(obj1); // 通过
  expect(obj).toEqual(obj1); // 通过
});
```


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


## 一些使用方法

### 测试随机数
在 Jest 中，可以使用`toContain`的方式测试随机数，如：
```javascript
it('使用toContain测试随机数', () => {
    expect([1, 2, 3, 4, 5]).toContain(Math.ceil(Math.random() * 5));
});
```
查看随机的值，是否在数组中，如果在的话，则通过。

也可以使用 mock 函数模拟随机数生成器的行为，以便您可以测试随机数的结果。下面是一个示例：

```javascript
// 测试函数
function getRandomNumber() {
  return Math.random() * 100;
}

test('测试随机数', () => {
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

### 验证数组长度及数组中的元素

```javascript
it('从排序数组中删除重复项', () => {
    const nums = [1, 1, 2, 2, 2, 3, 4, 4, 5];
    // 去重的函数，元素的 相对顺序 应该保持 一致 。然后返回 nums 中唯一元素的个数
    const length = removeDuplicates(nums);
    expect(length).toBe(5);
    expect(nums.slice(0, length)).toEqual([1, 2, 3, 4, 5]);
  });
```

## 扩展方法

### 检查一个值是否与一组给定的值中的至少一个相等(toBeOneOf)

如果检查数组中包不包含某个值可以用 `toContain`，例如：
```javascript
it('判断[1,2]中包不包含1', () => {
  expect([1, 2]).toContain(1);
});
```

但是要判断一个值，是否在数组中的话，就需要方法扩展了

```javascript
// 自定义 toBeOneOf 方法
expect.extend({
  toBeOneOf(received, expectedArray) {
    const pass = expectedArray.includes(received);
    if (pass) {
      return {
        message: () => `expected ${received} not to be one of ${expectedArray}`,
        pass: true,
      };
    } else {
      return {
        message: () => `expected ${received} to be one of ${expectedArray}`,
        pass: false,
      };
    }
  },
});

it('判断1在不在数组[1,2]当中', () => {
  expect(1).toBeOneOf([1, 2]);
});

```

