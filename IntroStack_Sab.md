# What is a stack? 

A Stack is a a data structure with Last-In First-Out behavior. What this means is whatever you put into it last is what comes out next. You can often imagine it like a stack of plates or papers. Often, a stack will only allow you to access the top-most element it contains. Any deeper elements will typically be hidden from the "user". It's important to note that the
"user" in this case is another programmer. They seek to use a Stack as a way to organize data in their project. 

A Stack typically supports two basic operations: push and pop. Push places data onto the Stack, like placing a plate at the top. Pop removes an item from the top of the Stack and 
returns it, like taking a plate off the Stack. A properly implemented stack is perfectly usable with just these functions. These functions typically exist under an object which tracks all of the Stack's internal data. 

Consider this example:

```Java
Stack my_stack = new Stack(); // we'll say that this stack accepts integers,
                              //    typically the Stack would either be for Object
                              //    types, or would be a Generic <T> type. 

my_stack.insert(13);
my_stack.insert(11);
my_stack.insert(5);
my_stack.insert(9);
my_stack.insert(140);

Integer item = my_stack.pop();
while (item != null){
    System.out.println(item);
    item = my_stack.pop(); // returns "null" when empty
}

```
Outputs
> 140

> 9

> 5

> 11

> 13

You can see that usage was pretty straightforward there. The idea itself is relatively simple. Now think, how might we achieve that behavior? Attempt to solve this on your
own. Consider how we can keep track of the top object, and all objects below it. 

There's a few ways we can implement this. First, if we're okay with out Stack having a maximum size, we could use an array. We could hide the array within the object, and 
keep track of the index that represents the top of the Stack. If the index goes beyond the length of the array, we just reject any "push" operations. But this implementation would be slow, 
annoying, and would naturally have a cap. Regardless, try to implement a Stack by using:
- An index of the stack's top
- A private array
- public push and pop methods



An alternative to using an array is to use references. References simply store the information about another object. The idea here is that if we have two different sets of objects,
for instance the Stack class and the StackObject class, we can have the Stack class reference only the top StackObject, and the StackObject class reference the next StackObject Object in line.

Then whenever we insert or pop something, we just update these references. So let's use an arrow to represent references. If A references B, we would write:

```
A -> B
```

So if we made a chain of references it would look like this:

```
A -> B -> C -> D -> E -> ... -> Z
```

The point of note above is that each object can only see the next object. They don't need any knowledge of any other objects. Now consider if our Stack referenced at the top
object in this chain. 

```
Stack |-> A -> B -> C -> D -> E -> ... -> Z
```

So the "top" of the Stack would be A. This means if we "push" something like "&" onto the stack it looks like this:

```
Stack |-> & -> A -> B -> C -> D -> E -> ... -> Z
```

And if we "pop" the stack twice it looks like this:

```
Stack |-> B -> C -> D -> E -> ... -> Z
  : returned "&" and "A"
```

If we pop it again:

```
Stack |-> C -> D -> E -> ... -> Z
  : returned "B"
```

So now we've seen a Stack in operation. We understand that we can use a chain of references, and we understand that this chain of references can be updated. But how do we create a reference?

A reference in a language like Java would look something like this:

```Java
class Referencer {
    Object next;

    Referencer(Object referencedThing){
        this.next = referencedThing;
    }

    Referencer(){
        this.next = null;
    }
}
```

The ".next" field of the objects created from this class would be the referenced objects. So to create this:

```
A -> B
```

We would do this:

```Java
Referencer B = new Referencer(); // empty constructor, .next == null

Referencer A = new Referencer(B); // This now references B, so A -> B -> null
```

Try to reproduce this behavior for a Stack and StackObject class in Java on your own. If you can't, take a peek at the example. 


```Java
class StackObject {
    private StackObject next; // ensures we're referring to another StackObject.
    private Object data; // We want to store data in these objects, this let's us to that.
                  // We could also use an int field, or a string, or something else.    
}

class Stack {
    private StackObject topObj; // we only need to track the top object!

    // We could also have optional length fields, ie: int length;
}

```

Given the above, or using what you created, reproduce the following methods:
 - Stack
   -   public <type> pop
   -   public void push

It should be noted that typically when popping from the stack, one returns the data in the object instead of the object itself. 
 
