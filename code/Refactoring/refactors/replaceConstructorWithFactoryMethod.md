# [Replace Constructor with Factory Method](https://refactoring.guru/replace-constructor-with-factory-method)

If constructing an object involves more than just instantiating an object, use a factory method. Another added functionality is that returning an object will be **optional**; it no longer strictly returns the object and can choose to return null. 

Consideration: separate it into a factory class if
- There are extra logic that are external to the created object.
- There is "choice". Example: if the user chose "urgent" then make an urgent one.

Relation: [[factoryMethod]] (design pattern)