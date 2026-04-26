# Builder

See `StringBuilder`.

A Builder class introduces a way to build classes bit-by-bit.

See a `Quest` class, which has subclasses `DailyQuest`, `UrgentQuest`... They all are by principal the same, but have several differing components. 
With a `QuestBuilder` class, I could build it per field via `qb.setName(...)`, `qb.setReward(...)`. I can also choose which type of `Quest` it is, incorporating use of an enum that lists each quest type.

Once finished, I can `qb.toQuest()`. Or `to(Specific)Quest`? 

The example shows just a `House` class that may have varying contents. One can have a swimming pool, another has a garage, and so on. Not subclasses of `House`. Does this mean what `Quest`needs should be a [[factoryMethod]]?

Recommended structure: 
- If the program has some specific rules, such as the order in which the fields need to be set (which ideally shouldn't be the Builder's concern) you can have a Director class that can abstract the implementations of these rules so that the client doesn't have to concern themself with it. 
### My thoughts

>What's so different with plain constructors?

Hypothesis:
- Less parameters when constructing. When a parameter isn't needed, there's no need to set it to `null` or `false`, just set needed ones.
- is that it?

> When setting data via user input, can and should I delegate validation to the Builder? Should I pass raw Strings into the Builder for it to parse?

Hypothesis: This validation will be way more pleasing, considering that it will be separated from **everywhere else**.
However, this probably sits about two (three) different cases:
- Validate each SET of inputs, re-ask the whole form if invalid.
  Validate upon `qb.toQuest()`, or something.
- Validate each input, when an input is invalid, re-ask the whole form.
  Simply throw an exception for an invalid field. When an exception is caught, loop the thing.
- Validate each input, but re-ask just that field.
  Vincent approached this by having the INPUT METHOD handle the validation. 
Another issue: a validation relies on a specific class; for example, "no duplicate usernames" validation needs the session class.