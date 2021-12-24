# 源码名称对应关系

element => component
container => dom element
fiberRoot => FiberRootNode
current => rootFiber => FiberNode
children => component

# 关于 component 何时 rerender

当 parent rerender 时，

child 为 class component 的，是否 rerender 由 shouldUpdate 决定，shouldUpdate 由两个因素决定：

1.  forceUpdate 强制更新
2.  shouldComponentUpdate 是否更新

child 为 function component 无论如何都会 rerender。

child 为 memo component 的，会根据 compare 决定是否 rerender。

# why state is a snapshot?

这个只在 function component 中成立，因为 useState 是不可变的，每次都是替换更新的。

在 class component 中，state 是 shallow merge 的，使用 object.assign 更新的同一个对象。

# useState

reducer(currentState, action)

useState 其实是特殊的 useReducer，其 reducer 为 basicStateReducer，

```

function baseStateReducer(state, action) {

return typeof action === ‘function’ ? action(state) : state;

}

```

# batch rendering

react 18 之前只有在 event handler 里面会 batch render，18 之后所有位置默认开启，如果要 opt out，需要用 flushSync

https://github.com/reactwg/react-18/discussions/21