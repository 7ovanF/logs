# Factory Method

This exists to give both the Single Responsibility and Open/Closed Principles. It loosens up coupling between the Client and Product class by separating the construction code from the Client.

(see [[replaceConstructorWithFactoryMethod]])
Not to be confused with a Factory class, which is just a normal class handling extra construction logic
Built upon the "factory method", where construction logic is separated from the Client to promote SRP. However, the factory class itself is still open to changes.
Actually do i need to write this here? I do understand the structure now.

### My thoughts

> So... this literally **cant** be implemented for classes that force the Main I/O to adapt according to the inputs?

So, 

> Wait, Open/Closed Principle came back, this time to the Client. It needs to choose which ConcreteFactory to use.
> ... what the fuck, we solved SRP but other than that went back to square one! Wait, since the Client passes arguments to the factory the exact same way the normal constructor needs it, the factory is basically just a middle man???

Well... I guess, if the Concrete Factories have extra logic that the Client shouldn't be concerned about...

> Then just use normal Factory classes?

yea what the fuck... lets continue at an uncertain "later"