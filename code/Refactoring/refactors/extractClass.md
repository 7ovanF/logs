# Extract Class

Extract functionality when it looks very not Singly Responsible.

This will tolerate change more.

This is... very subjective. One would extract, one would keep it in the same class (with inline class).
It probably boils down to something like this:
```
Less			Functionality of Extracted Class		 More
<----------------------------------------------------------->
Inline class									Extract class
```