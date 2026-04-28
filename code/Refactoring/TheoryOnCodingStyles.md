Points I caught:
- YAGNI (Ye Ain't Gunna Need It)
  I might be slightly biased towards NOT heeding this, but, this has a very good point especially if the considered-to-be-overengineered component **is not complex at all to start with**. 
  Personally I'd immediately overengineer if it's slightly complex.

What if there is a coding style, where even if I refactor wildly, little is changed?
A very uncoupled system?
Where if i switch to using a class to represent invalid input formats instead of exceptions, i only change the thrower and the handler, but not the user interface code?

"Ignorance is a Bliss" style.

Consider this scenario:
We only need a status String over in Main. (Wait, no, it also needs an indicator whether it succeeded or not to determine whether to repeat or not. Wait no, technically not even that; whether it fails or not the input will loop to the beginning of the form. However, that's going to be very specific-cased and the behavior will be very hard to refactor.)

Behavior.

That's it for now, back to patterns