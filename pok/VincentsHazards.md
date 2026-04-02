## Structural Hazard
When your mom wants to make fried bananas, she needs to use the stove. While she is dipping them in the batter, your dad wants to boil potatoes. So, although both of them started at different times, they both need to use the stove at the same time. The solution is to **stall**, so your dad can wait a minute before boiling the potatoes.

Interuption: OR... 

Dropping the analogy thing going on, one #TODO case that involves structural hazard is the IF/MEM crash. Two operations read from the memory at the same time, even though it is only prepared to WRITE and READ on the same clock (with RAW: read-after-write hazard; which is a data hazard, not structural. Write on the first half of the clock, read on the second).  

It is solved by **separating the memory into the instruction and data memories**. "Whatever the hell happened to Von Neumann?" you might ask. In the MIPS ISA, both *do* use the same memory (educational diagrams are a different matter). But, on the real implementation level, MIPS CPUs have both instruction cache and data cache; this means, the two READs can be done simultaneously; no stall needed. The actual memory (RAM) itself is still one and the same.

MIPS is designed to be implemented with the modified Harvard architecture. <- you should revalidate this.

## Data Hazard
When your mom wants to make fried bananas, she needs to wait for you to get back from the store. The solution is to throw the bananas from the market to her kitchen with a **catapult** (**forwarding**).

Case 2: You're still paying at the cashier, but your mom is about to turn on the stove! You call your mom and tell her to "WAITTTTTTTT". So your mom waits for 3 picoseconds, enough time for you to ready your catapult.

**Better Analogy**
I want to make onion rings. You are making mozzarella sticks. instead of waiting for you to finish frying, you can **forward** me the batter immediately after you used it.

Case 2: The batter... something... idk
## Control Hazard
Your mom asks you to buy bananas if your dad comes home to make dinner. But, if you wait your dad to come home before buying the bananas, you wont get it in time for dinner. The solution is to make a **prediction** that your dad will come home. Everyday, he comes home. So, the chances that he doesn't is slim. Even if the prediction is wrong, you can still eat the bananas yourself ;)