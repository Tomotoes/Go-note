# Sync

sync.map，它是原生支持并发安全的map。虽然说普通map并不是线程安全（或者说并发安全），但一般情况下我们还是使用它，因为这足够了；只有在涉及到线程安全，再考虑sync.map。

但由于sync.Map的读取并不是类型安全的，所以我们在使用Load读取数据的时候我们需要做类型转换。