# Introduction #

The library does not cover all the Java regex functionality yet, so you may need to extend the library to add a feature you need.

For example, lets write a pattern component that matches ASCII control characters.

We must define a class that implements the [PatternComponent](http://code.google.com/p/hamcrest-text-patterns/source/browse/trunk/src/org/hamcrest/text/pattern/PatternComponent.java) interface.


```
public class AsciiControlCharacter implements PatternComponent {
    ...
}
```

Then, we must implement the {{buildRegex}} method to append the regex syntax for the pattern component to the StringBuilder.

```
public class AsciiControlCharacter implements PatternComponent {
    public void buildRegex(StringBuilder builder, GroupNamespace groups) {
        builder.append("\\p{Cntrl}");
    }
}
```

(We can ignore the groups parameter.  That's used by the `capture(...)` and `valueOf(...)` pattern components to identify capture groups by name.)

Finally, we should write a static function to make patterns that use this component easy to read.  For example, we could put factory functions for all the pattern components we need for our project in a single class and statically import them into the classes that use them.

```
public class ProjectSpecificPatterns {
    public static PatternComponent asciiControlCharacter() {
        return new AsciiControlCharacter();
    }
}
```

That's it.