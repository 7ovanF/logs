# Built-in Exception

This is just my theory.

Instead of complicatedly returning validation results, the status and its message, even though "expected" and explicitly handled, should just be returned as an exception (contrary to popular opinion)

Having to handle these exceptions will be part of using the API! These exceptions come WITH it, so deal with it! (ha! because you're gonna... DEAL WITH... exceptions!)

Use case: when status returning isn't complicated, like only needing to signify a failure with a message. Also, use only if performance isn't a concern.

However, also: if the try catch encompasses way too much of the program, it would be a major problem. Try to minimize exception usage.