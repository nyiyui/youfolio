# Object-oriented Programming (OOP)

## Why OOP?
OOP is a philosophy for organising programs, similar to procedural and functional progamming.

## Objects
In OOP, all actions that the program does is organised around the "object".
An object represents a state machine, or an abstraction that protects its internal state from corruption.
It primarily does this by limiting access to internal state: objects only allow methods (written for the class) to edit its internal state, allowing checks to be performed on the state modification.

## Classes
Althought classes are a staple of OOP, classes are not necessarily a first-class citizen for some OOP implementations, such as [Io](https://iolanguage.org)'s object system.
Classes can be thought of as a template to create objects. In languages like Java, an object can only be made using a class as a template (called instantiation).

```java
public class SMTPMailSender {
    public void sendMail(Mail[] ms) { /* ... */ }
}
```

## Abstract Classes and Interfaces
There seems to be some confusion between the two, as they are both very similar.
The main differences are:
- a class can implement multiple interfaces, but can only inherit one abstract class, and
- abstract classes can have their own state (i.e. instance variables) (interfaces cannot).
Interfaces are useful when you may have an implementation that implements multiple interfaces.

An example of an interface is the `Iterable<T>` interface (pseudocode):
```java
public interface Iterable<T> {
    Iterator<T> iterator();

    default void forEach(Action<? super T> action) {
        for (T t : this) action.apply(t);
    }
}
```
(`? super T` means a type that is `T` or a superclass of `T`.)

A list class may implement `Iterable<T>` and also implement `Comparable<T>`. An abstract class would not allow multiple interfaces, so interfaces are used here.

Abstract classes are useful when you want to have internal state. An example is the `Writer` class (pseudocode):
```java
public abstract class Writer {
    private char[] writeBuffer;
    private static final int WRITE_BUFFER_SIZE = 1024;
    public abstract void write(char buf[]) throws IOException;
}
```

```java
public 
public abstract class MailSender {
    public abstract void sendMail(Mail m) { }
    public void sendMail(Mail[] ms) {
        // ignoring error handling (specifically: handling when some mail fails)
        for (Mail m : ms) {
            sendMail(m);
        }
    }
}
```

## Visibility
This is important in OOP as one of the main ideas is to prevent outsiders (e.g. sleep-deprived programmers) from making mistakes (e.g. by setting an invalid file descriptor).

Java provides a visibility system separating classes hierarchy from each other. These visibility modifiers can be put in a declaration to extend visibility from the class itself to:
- `public`: anything
- nothing: anything in the same package
- `protected`: any subclass
- `private`: nothing

Private attributes are useful for storing easily-corrupted internal state. (I don't see a point for public class/instance variables, as they can also be corrupted.)

Some languages, such as Ruby or Clojure, provide instance-private methods where the object can only accces its own attributes, but not of another instance of the same (or sub-) class.

```ruby
class Class
  private
  def priv_method(other)
    other.priv_method # errors with NoMethodError (private method `priv_method' called for #<Class:0x...>)
  end
end
```

## UML
UML is a visual language used to describe class hierarchies in object-oriented systems.