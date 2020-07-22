---
layout: post
title: "Android: Kotlin and Parcelable objects and arrays"
date: 2015-12-06
comments: true
---
[Kotlin](https://kotlinlang.org/) is current in the Version 1.0 Beta 2 (**Update**: Version 1.0 Beta 3 is out) and is just before the final Version 1.0. So I'm going to take a closer look how well Kotlin is working with Android. The reasons why Kotlin could be a good replacement to the good old Java can be found here: [The Kotlin Language: 1.0 Beta is Here!](http://blog.jetbrains.com/kotlin/2015/11/the-kotlin-language-1-0-beta-is-here/)

But a few big points:

- Null-Safety
- Extensions function
- Lambdas
- Named parameters and optional parameters
- Easy to learn

The integration with Android-Studio is great because Kotlin is made by JetBrains. The company behind the IntelliJ IDEA and of course Android-Studio. There is also a code converter from Java to Kotlin and Kotlin can call Java code and otherwise.

But sometimes I struggle with Kotlin and the Android API like I want to show in this post: Parcelable Object with Arrays.

I have the following model:

```kotlin
data class Timekeeper(var name: String, var time: String)
```
And:

```kotlin
data class TimekeeperHolder(var timekeepers: Array<Timekeeper>)
```
   
(Just a simple example ;) )

But now I want that both classes are parcelable. Without the Array in TimekeeperHolder it would be straight forward but with this array the CREATOR in Timekeeper must be visible to TimekeeperHolder.

The Timekeeper:
```kotlin
data class Timekeeper(var name: String, var time: String) : Parcelable {
    override fun describeContents() = 0

    override fun writeToParcel(dest: Parcel, flags: Int) {
        dest.writeString(name)
        dest.writeString(time)
    }

    companion object {
        val CREATOR = object : Parcelable.Creator<Timekeeper> {

            override fun createFromParcel(`in`: Parcel) =
                Timekeeper(`in`.readString(), `in`.readString())

            override fun newArray(size: Int): Array<Timekeeper?> =
                arrayOfNulls(size)

        }
    }
}
```


With the companion object it's possible to declare an object inside a class and the member inside can be called like: Timekeeper.CREATOR.
	
The TimekeeperHolder needs to write the array into a typedArray and when we want to create the object from the parcel we need to create the array from a typedArray:

```kotlin
data class TimekeeperHolder(var timekeepers: Array<Timekeeper>) : Parcelable {
    override fun describeContents() = 0

    override fun writeToParcel(dest: Parcel, flags: Int) {
        dest.writeTypedArray(timekeepers, flags)
    }

        companion object {

            val CREATOR = object :
                Parcelable.Creator<TimekeeperHolder> {

            override fun createFromParcel(`in`: Parcel) 
                = TimekeeperHolder(
                    `in`.createTypedArray(Timekeeper.CREATOR)
                     )

            override fun newArray(size: Int): Array<TimekeeperHolder?> 
                = arrayOfNulls(size)
        }
    }
}
```
	
With the given constructor we can easily return the object in createFromParcel(\`in`: Parcel) and the createTypedArray method only needs the CREATOR from the Timekeeper object.