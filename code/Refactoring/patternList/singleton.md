# Singleton

Quite simply, a class that **strictly** can only be instantiated once.

That describes quite a lot, and also, it implies that this is a very situational pattern. This should only be applied when a class is MOST certainly going to be unique (say, a database, an OS-level lock manager, or a single hardware interface).

Actually, there's a second problem a Singleton may or may not fix. That is, providing a global access point to that unique instance.

How to use: privatize the constructor (you can't use it externally since it strictly needs to return the object, therefore uncontrollable), then make a custom constructor which controls use of that private constructor.
Something like:
```java
private Singleton instance;

private Singleton() { 
    // maybe some logic here 
}

public getInstance() { // oh, this is why it's a "global access point"
    if (instance == null) {
        instance = new Singleton();
    }
    return instance;
}
```

My impression is that this is surprisingly popular.