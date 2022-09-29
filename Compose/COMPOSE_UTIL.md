### Compose Lifecycle Observer

```
@Composable
fun ComposableLifecycle(
    lifeCycleOwner: LifecycleOwner = LocalLifecycleOwner.current,
    onEvent: (LifecycleOwner, Lifecycle.Event) -> Unit
) {
    DisposableEffect(lifeCycleOwner) {
        val observer = LifecycleEventObserver { source, event ->
            onEvent(source, event)
        }
        lifeCycleOwner.lifecycle.addObserver(observer)
        onDispose {
            lifeCycleOwner.lifecycle.removeObserver(observer)
        }
    }
}
```

### Compose IfTrue Modifier extension function
```
inline fun Modifier.ifTrue(
    value: Boolean,
    builder: () -> Modifier
): Modifier {
    return then(if (value) builder() else Modifier)
}
```
