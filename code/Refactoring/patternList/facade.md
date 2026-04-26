# Facade

When the program generally just needs a subset of a very sophisticated library, make a Facade class that hides the details and just does a specific order of operations. Not only abstraction, if a lot of classes depend on this, this would be a good way to minimize changes (e.g. if the library was updated and doesn't work the same, you only need to change the Facade). Don't make a god facade, though.

You can also make multiple facades that use each other if their methods are quite too unrelated to combine into a single facade.

Keep in mind, there's not really a need to do this if only single method calls are done (unnecessary middlemen).

Usually, this is just for programs involving 3rd party libraries. But, wouldn't this apply to java packages too?