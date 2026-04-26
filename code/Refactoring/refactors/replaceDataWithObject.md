# [Replace Data Value with Object](https://refactoring.guru/replace-data-value-with-object)

Subtype of [[extractClass]].

Extract certain data (especially primitive ones), or better yet, along with the behaviors that relate to it. Sometimes it may NOT be needed if that property realistically only belongs to one class, but when multiple classes would use that class and you know it, it's a very good idea.
When a data gets expanded upon, you'd probably not get the chance to avoid shotgun-surgery if it's a primitive and it gets leaked around the program. Just emphasizing to myself to be careful of spreading primitives around, and that **specialized data objects are generally a great idea because changes will only be done to that object**. 

>Personally, I'm an advocate of over-engineering. Call me paranoid. "Ah, there's no need at all for this class... just use an array..." well, have fun refactoring when the time comes to complicate that field.
>
>I think that, arrays, lists, and hashmaps should only be used if it's **strictly** an array, a list, or a hashmap. Otherwise, if it has SLIGHT extra behavior, immediately extract to a class.
>
>Oh right, "That's adding extra complexity to the code!" blah blah, have fun making the client operate very carefully on that morbidly ambiguous and specific-behaviored Hashmap. 

(+) Constants like `ROLE_ADMIN` or `DIFFICULTY` should probably be part of an enum.