https://refactoring.guru/replace-type-code-with-class
https://refactoring.guru/replace-type-code-with-subclasses

Type code: a code, like a boolean or a String, that determines how a class behaves.

> Note: apparently, the website says to turn the constants into a class, not separate object itself to its own class? I don't fully understand this one.

It could've been determined by separated classes/subclasses; more commonly a subclass.

Note: this is for classes that realistically CAN be separated without complications (e.g. that type code doesn't change at all post-instantiation). If it does, this is not applicable.