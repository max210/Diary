自定义 react hooks

```javascript
// 获取最新的 state
export function useNewState(state) {
  const stateRef = useRef(state)

  useEffect(() => {
    stateRef.current = state
  }, [state])

  return stateRef.current
}

// 获取最新 state 计算后的值
export function useComputedState(state, computeFunc) {
  const stateRef = useRef(state)

  useEffect(() => {
    stateRef.current = computeFunc(state)
  }, [state])

  return stateRef.current
}
```

