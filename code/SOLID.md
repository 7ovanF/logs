# SOLID
> unfortunately not applicable to TP2

## 1. Single Responsibility Principle (SRP)
**A class should have only one reason to change.**
> reason is confusing.
> Gather together the things that change for the same reasons.
> Separate those things that change for different reasons.

The accountant and secretary might be the same person, but the "account" and "secretary" module should be separated.

SRP is an application of SoC (Separation of Concerns) in the scope of OOP.

## 2. Open/Closed Principle
**Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification.**
This does not mean "no changes at all", but only little change should happen. 
## 3. Liskov's Substitution Principle
**Derived or child classes must be able to replace their base or parent clases.**
Literally what `extends` represents! It shouldn't make strange changes to the parent's method that makes it behave very differently. If a class can not mimic its parent, it shouldn't be its child in the first place.
## 4. Interface Segregation Principle
(for interfaces)
**Do not force any client to depend on methods which is irrelevant to them.**

## 5. Dependency Inversion Principle
**High-level modules should not depend on low-level modules. Both should depend on abstractions.**
