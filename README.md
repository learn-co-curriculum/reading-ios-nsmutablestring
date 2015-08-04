# `NSMutableString`

## Objectives

1. Recognize the value of a mutable string.
2. Create a new mutable string variable.
3. Concatenate to a mutable string with the `appendString:` and `appendFormat:` methods.

## Static vs Mutable

The `NSString` objects that you've worked with so far are called "static" strings. In order to be altered at all, a static string has to be completely overwritten; this how the `stringByAppendingString:` method works. It actually returns a newly-created string that's the concatenation of argument string to the recipient string of the method call. 

In our examples that you've seen, most of the time we've just captured that return into the same string variable that we called the method on, but this isn't required when calling the method. It also isn't assured by the method that this will be the behavior every time.

Wouldn't it be nice, if we had a string that we could just tack substrings onto without having to captur a return string every time? Well, we're in luck because that's exactly what `NSMutableString` is for!

## Creating an `NSMutableString`

Because the string literal (`@""`) creates `NSString` variables, we can't directly use it to create an `NSMutableString`. Instead, we have to use methods. To create an empty mutable string, use the `+alloc` and `-init` methods:

```objc
NSMutableString *mWelcome = [[NSMutableString alloc] init];
```
You can create a mutable form of a static `NSString` object by calling the `mutableCopy` method on it. You can do this with either a string literal or submitting an existing string as the argument:

```objc
NSMutableString *mWelcome = [@"Welcome" mutableCopy];

NSLog(@"%@", mWelcome);
```
OR

```objc
NSString *welcome = @"Welcome";
NSMutableString *mWelcome = [welcome mutableCopy];

NSLog(@"%@", mWelcome);
```
Both of these will print: `Welcome`.

### Always Initialize Mutable Strings

Forgetting to initialize a mutable string is an easy oversight in programming Objective-C that can be a difficult bug to diagnose. In this case, the mutable string can be the recipient of method calls but won't actually retain any of the substrings submitted to it. Attempting to access the contents of the mutable string at a later point (such as printing it with an `NSLog()`) will return `nil`.

```objc
// bad example

NSMutableString *welcome;

[welcome appendString:@"Welcome"];

NSLog(@"%@", welcome);
```
This will print: `(null)` (which is how `nil` is expressed by LLDB in the debug console).

Oops.

## Concatenating With An `NSMutableString`

An `NSMutableString` shines best when used in combination with loops or `if` statements. Not having to capture a returned string means that you can't accidentally save the return into a different string variable. It's also a little easier to read the code to interpret the intention of the command. Compare these new methods to the `stringByAppendingString:` and `stringByAppendingFormat:` methods on `NSString`.

### The `appendString:` Method

The `appendString:` method is a `void` return method that concatenates the argument string directly onto the mutable string on which the method is called:

```objc
NSMutableString *welcome = [[NSMutableString alloc] init];

[welcome appendString:@"Welcome"];

NSLog(@"%@", welcome);
```
This will print: `Welcome`.

But if we continue appending additional words with this method, we'll get a concatenated string that won't read quite correctly:

```objc
[welcome appendString:@"to the"];
[welcome appendString:@"Flatiron School!"];

NSLog(@"%@", welcome);
```
This will print: `Welcometo theFlatiron School!`.

Hmmm, that's not right.

### The `appendFormat:` Method

However, the `appendFormat:` works in a similar way but accepts an *interpolated* string as the argument to concatenate:

```objc
NSString *toThe = @"to the";
NSString *flatironSchool = @"Flatiron School!";

NSMutableString *welcome = [@"Welcome" mutableCopy];

[welcome appendFormat:@" %@", toThe];
[welcome appendFormat:@" %@", flatironSchool];

NSLog(@"%@", welcome);
```
This will print: `Welcome to the Flatiron School!`.

