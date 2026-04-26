# Which of Your Code... Smells?
(main source: refactoring.guru)
Cues that you should do something about your code.

> Note: ignore these if the pattern purposefully violates these. Pattern > ambiguous "smell" potential
## 1. Bl\*ated Code
- Long methods
  > Small methods tend to live longer.
- Large classes
  Deodorant: [[extractClass]], [[extractGeneralized]]
- [Primitive obsession](https://refactoring.guru/smells/primitive-obsession)
  see [[replaceDataWithObject]] 
  Funnily, I see this being very relevant with coupling if said primitive is accessed outside the class (which, if it behaves complexly, it shouldn't in the first place).
- a
- EXTRA FROM ME: Constant Hell
  If, say, a String message is probably only used once in the whole lifetime of the program, don't consider extracting it into a constant just yet (hard-coding it is enough). Unless there's a language system or something, haha.

Let's see if this one is useful...
# 5. Co\*plers 
- [Feature Envy](https://refactoring.guru/smells/feature-envy) (idk about this one, it might hurt my flexibility since its more semantic than coupling)
  A class accesses the data of another class more than its own. 
  Why it happens: maybe when data is extracted to another class.
  Why it smells: shouldn't that class handle its own data? (violates encapsulation)
  Deodorant: your choice, but... [move method to the class that owns it / uses it more](https://refactoring.guru/move-method) or [extract from that method the part that needs the data](https://refactoring.guru/extract-method)
  BUT: I mean, if the way the data is being handled really isn't the owner class's concern, then go ahead.
- [Inappropriate Intimacy](https://refactoring.guru/smells/inappropriate-intimacy)
  When two classes know each other, intimately (internal values and methods), too much
  Why it smells: Do you need to ask? Jeez, put your dick away.
- [Message Chains](https://refactoring.guru/smells/message-chains)
  Client needs to explicitly chain and know where to navigate through methods.
  Hide delegate, or the client directly calls endpoint (VALID? SEE BELOW).
- [Middle Man](https://refactoring.guru/smells/middle-man) (just bloat)
  Class does nothing other than call the other class.
  Hidden Delegates too much? This may happen. 
  Deodorant: [Remove it](https://refactoring.guru/remove-middle-man)
  Note: in cases like a concrete object that explicitly OWNS a field, it should become a middleman (like there's any other way, lmao).

**Important things i found:**
- See [Hide Delegate](https://refactoring.guru/hide-delegate). This implies a bit of smell about having Main call a Builder directly.