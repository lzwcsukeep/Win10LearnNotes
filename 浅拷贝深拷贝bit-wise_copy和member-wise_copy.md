**Member-wise Copy**

Is when you visit each member and explicitly copy it,<u> invoking its copy constructor.</u> It is the proper way of copying things. If done right, it is tantamount to a deep-copy, because each member whose copy constructor you invoke will (or should) in turn perform member-wise copy of its own members, and so on. The opposite is bit-wise copy, which is a hack, see below.

**Bit-wise Copy**

Is a specific form of shallow copy. It is when you simply copy the bits of the source class to the target class, using `memcpy()` or something similar. <u>Constructors are not invoked, so you tend to get a class which *appears* to be all right but things start breaking in horrible ways as soon as you start using it. </u>This is the opposite of member-wise copy, and is a quick and dirty hack that can sometimes be used when we know that there are no constructors to be invoked and no internal structures to be duplicated. For a discussion of what may go wrong with this, see this Q&A: [C++ bitwise vs memberwise copying?](https://stackoverflow.com/questions/15123516/c-bitwise-vs-memberwise-copying)

**Shallow Copy**

Refers to copying just the immediate members of an object, without duplicating whatever structures are pointed by them. It is what you get when you do a bit-wise copy.

(Note that there is no such thing as "shadow copy". I mean, there is such a thing, in file systems, but that's probably *not* what you had in mind.)

**Deep Copy**

Refers to not only copying the immediate members of an object, but also duplicating whatever structures are pointed by them. It is what you normally get when you do member-wise copy.

**To summarize:**

There are two categories:

- Shallow Copy
- Deep Copy

Then, there are two widely used techniques:

- Bit-wise Copy (a form of Shallow Copy)
- Member-wise Copy (a form of Deep Copy, if done right.)

As for the hear-say about someone who said something and someone who said something else: bit-wise copy is definitely always shallow copy. Member-wise copy is usually deep copy, but you may of course foul it up, so you may be thinking that you are making a deep copy while in fact you are not. Proper member-wise copy relies on having proper copy constructors.

Finally:

The default copy constructor will do a bit-wise copy if the object is known to be trivially copyable, or a member-wise copy if not. However, the compiler does not always have enough information to perform a proper copy of each member. For example, a pointer is copied by making a copy of the pointer, not by making a copy of the pointed object. That's why you should generally not rely on the compiler providing you with a default copy constructor when your object is not trivially copyable.

A user-supplied constructor may do whatever type of copy the user likes. Hopefully, the user will choose wisely and do a member-wise copy.
































