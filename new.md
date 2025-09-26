# new

## new 表达式的总统流程

```text
// 语法：new F(arg1, arg2, ...)

NewExpression(evalTarget, argList):
  Fref = Evaluate(evalTarget)
  F = GetValue(Fref)

  // 关键：只有“可构造”的函数对象才能被 new
  if IsConstructor(F) == false:
    throw TypeError("F is not a constructor")

  // newTarget 默认就是 F（可被子类/Reflect.construct改变）
  return Construct(F, argList, newTarget = F)
```

这里的分水岭就是 IsConstructor(F)。它本质上就是是否存在内部方法 `[[Construct]]`：

```text
IsConstructor(obj):
  return obj has an internal method [[Construct]]
```

普通函数对象通常同时拥有 `[[Call]]`（可被当作函数调用）和 `[[Construct]]`（可被 new）。

箭头函数对象只有 `[[Call]]`，没有 `[[Construct]]`；因此 IsConstructor(箭头函数) == false，直接抛 `TypeError`。

## 普通函数的 \[\[ Construct ]] 如何工作

当 `IsConstructor(F)` 为真时，`new F(...)` 会走 `Construct(F, args, newTarget)`。对“普通函数对象”（ordinary function object），它的构造流程大致如下：

```text
Construct(F, args, newTarget):
  // 1. 取原型：优先用 newTarget.prototype，否则降级到 %Object.prototype%
  proto = GetPrototypeFromConstructor(newTarget, default = %Object.prototype%)

  // 2. 创建实例对象（即 future this）
  obj = OrdinaryObjectCreate(proto)

  // 3. 准备 this 绑定的函数执行环境（普通函数的 this 是动态的）
  //    注意：箭头函数没有这步 —— 它的 this 是“词法捕获”的
  thisValue = obj

  // 4. 以 this = obj 来“调用”函数体（走 [[Call]]）
  result = F.[[Call]](thisValue, args)

  // 5. 如果返回值是对象，则返回该对象；否则返回步骤 2 创建的 obj
  if Type(result) is Object:
    return result
  else:
    return obj
```

## 箭头函数为什么不能 new

箭头函数对象没有 `[[Construct]]`，只实现了 `[[Call]]`，并且其 `[[ThisMode]] = "lexical"`（this 来自定义时的外层词法环境）。因此：

```text
IsConstructor(arrowFunction)  // -> false
// 于是 new arrowFunction() 走不到 Construct，直接抛：
TypeError: arrowFunction is not a constructor
```
