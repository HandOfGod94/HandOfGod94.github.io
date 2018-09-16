---
layout: post
title: Salting - just cryptography or something else
---

Traditionally salting is used in preservation of food. As the name
suggests it performs similar task in computer
science also. So salting is basically a technique use to preserve
**password**. Ever though how can we use it in BigData Systems.

> Never ever ever ever try to **encrypt** passwords. Because if a
> thing can be encrypted then it can be decrypted also. **Always
> hash your passwords**.

So in terms of cryptography it is used as an extra input to hash
function. Salting parameters has to be predefined.

For e.g.  
Lets say your password is `world`, and hash function be 
`hash(str)`  
Assume, `hash('world')='hello'`,  
Now if we use *randomly generated salt*,  
`hash('salt','world')='$hash_salt$hash_password'`

Only you know that anything between `$..$` is salt so it becomes
hard to retrieve password for others.

> Note: `$..$` is just a representation. It can be chosen in anyway
> you like. It can be starting bytes, ending bytes etc.

## Salting in BigData

So where to use this? In order to understand lets first
understand a scenario:

Let there be a store **Wal-kart**.

Current Database design:

`Table Item(uniqueID Integer, price Float, brand String, time TimeStamp)`


* `buy(Item.uniqueID)`: buy any item

Assume, **uniqueID** is generated from hashing *brand* and *price* 
(lame!!, What were they thinking?)

Let's say they are strict in their customer policy. They don't
allow any items once sold to be returned or exchanged.
*Voila!!!* good news, CEO changes, and now modifies
customer-policy. They support now return and exchange of items. 

* `return(Item.uniqueID)`: defected items
* `exchange(Item.uniqueID)`: replace it with similar.

Now *Wal-kart* wants after how many days people generally return
defected items. But since `return` operation will generate same
`uniqueID`, they've no way of knowing the past data. Plus all the
new transations are also carried out on same design. Here we could
leverage `salting` technique to generate `uniqueID`. That way
you've just modified input to the hash function which generates
`uniqueID`.

The above scenario seems trivial, as if something which would never
occur. But in Data-Management, there are cases when you want
to process every data which passes through irrespective of how
similar they are.
