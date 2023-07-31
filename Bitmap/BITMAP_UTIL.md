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

### Compose Conditional Modifier extension function
```
inline fun Modifier.conditional(predicate: Boolean, onTrue: () -> Modifier, onFalse: () -> Modifier) =
    then(
        when (predicate) {
            true -> onTrue()
            false -> onFalse()
        }
    )
```

### Swipe gesture detecotor
```
sealed class SwipeDirection {
    object Left: SwipeDirection()
    object Right: SwipeDirection()
}

fun Modifier.horizontalSwipeGesture(
    onSwipe: (SwipeDirection) -> Unit
): Modifier = composed {
    val upperThreshold = 25
    val lowerThreshold = -25

    val offsetX = remember { mutableStateOf(0f) }

    pointerInput(Unit) {
        detectDragGestures(
            onDrag = { _, dragAmount ->
                offsetX.value += dragAmount.x
            },
            onDragEnd = {
                when {
                    offsetX.value > 0 && offsetX.value > upperThreshold -> {
                        onSwipe(SwipeDirection.Right)
                        offsetX.value = 0f
                    }
                    offsetX.value < 0 && offsetX.value < lowerThreshold -> {
                        onSwipe(SwipeDirection.Left)
                        offsetX.value = 0f
                    }
                }
            }
        )
    }
}
```
