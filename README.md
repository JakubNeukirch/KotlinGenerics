# KotlinGenerics
My description of various things to do with Kotlin generics and examples, feel free to add your usage examples.
Most of examples are made with use of reified classes but may also be done with use of standard KClass instances, for example instead of `T::class` (where T were provided as `Something` while calling function) you can use `Something::class`

## Calling companion object function
Firstly we need to take KClass instance of companion object which happens by `T::class.companionObject`, it may be null if class doesn't have companion object.
Later `companionClass` can be handled as typical KClass, so when we find function (`KFunction`) that we need we can simply call it `func.call()` but be aware, calling companion object's function requires passing companion object's instance as argument, even when the function doesn't have any arguments declared. You can access companion object's instace from `Something`'s KClass (not companion object's KClass).

In case companion object's function has arguments, they are passed after companion object's instance.

Example class:
```kotlin
class Something {
    companion object {
        fun staticTest(): String {
            return "Works"
        }
    }
}
```

Example usage calling:
```kotlin
    inline fun <reified T> callStatic(): String {
        val companionClass = T::class.companionObject!!
        val func = companionClass.functions.find {
            it.name == "staticTest"
        }!!
        return func.call(T::class.companionObjectInstance) as String
    }
```
