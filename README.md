# Hot-Cold-Observable

# Cold Observable
![Image of cold observable](https://cdn-images-1.medium.com/max/1600/1*mKI4mNHesE1bfXk6YJ_rEA.png)
- Observable only emit item when has subscribe
- It will emit again when has new subscribe
* Example:
```kotlin
var coldObservable = Observable.create<Int> {
    for (i in 0..2) {
        println("Source Emit $i")
        it.onNext(i)
    }
}

coldObservable.subscribe({
    println("onNext, subscriber1: $it")
}, {})
coldObservable.subscribe({
    println( "onNext, subscriber2: $it")
}, {})
```
* Result:
```kotlin
Source Emit 0
onNext, subscriber1: 0
Source Emit 1
onNext, subscriber1: 1
Source Emit 2
onNext, subscriber1: 2
Source Emit 0
onNext, subscriber2: 0
Source Emit 1
onNext, subscriber2: 1
Source Emit 2
onNext, subscriber2: 2
```
# Hot Observable
![Image of hot observable](https://cdn-images-1.medium.com/max/1600/1*gSwdDY0PoF0hUR0yGcNpZA.png)
- Observable emit item when created
- Observable doesn't again emit when has a new subscribe
* Example
```kotlin
var coldObservable = Observable.interval(1, TimeUnit.SECONDS)
    .doOnNext {
        println("Emitted $it")
    }
var connectableObservable: ConnectableObservable<Long> = coldObservable.publish()

connectableObservable.connect()

Thread.sleep(2000)

connectableObservable.subscribe({
    println("onNext, subscriber1: $it")
}, {})

Thread.sleep(2000)
connectableObservable.subscribe({
    println( "onNext, subscriber2: $it")
}, {}))
```
* Result
```kotlin
Emitted 0
Emitted 1
Emitted 2
    onNext, subscriber1: 2
Emitted 3
    onNext, subscriber1: 3
Emitted 4
    onNext, subscriber1: 4
    onNext, subscriber2: 4
```
# Create Hot Observable
