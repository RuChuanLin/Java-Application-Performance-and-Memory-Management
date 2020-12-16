# Finding out which methods are being compiled in our applications

## VM Options

Here is a `JVM flag` example 

```
-XX:+PrintCompilation
```

There are serveral parts of this statement :

1. `-XX` : meaning this is an advanced options
2. `+` : indicating if we want this option to be switched on(+) or off(-)
3. `PrintCompilation` : the option name

The statement is `case sensitive`, so XX can not be lower case, and for the options name, the first letter of each word shall be upper case

We can add this in configurations on Intellij

![](../img/2020-12-15-15-06-12.png)

## Terminal

We can expand our instruction in the last section

```
java -XX:+PrintCompilation Main 10
```

And then we will see lots of things

```log
     88    1     n 0       java.lang.System::arraycopy (native)   (static)
     89    3       3       java.lang.String::hashCode (55 bytes)
     89    4       3       java.lang.System::getSecurityManager (4 bytes)
     89    5       3       java.lang.String::equals (81 bytes)
     90    6       3       java.lang.String::getChars (62 bytes)
     90    8       3       java.io.WinNTFileSystem::normalize (143 bytes)
     90    7       4       java.lang.String::length (6 bytes)
     90    2       4       java.lang.String::charAt (29 bytes)
     90    9       3       java.lang.Object::<init> (1 bytes)
     91   10       3       java.lang.CharacterData::of (120 bytes)
     91   13       3       java.lang.Character::toLowerCase (9 bytes)
     91   14       3       java.lang.CharacterDataLatin1::toLowerCase (39 bytes)
     91   11       3       java.lang.CharacterDataLatin1::getProperties (11 bytes)
     91   15       3       java.lang.String::<init> (82 bytes)
     91   12       3       java.util.Arrays::copyOf (19 bytes)
     91   16       1       java.lang.ref.Reference::get (5 bytes)
     91   17       1       sun.instrument.TransformerManager::getSnapshotTransformerList (5 bytes)
     91   18       3       java.lang.StringBuilder::<init> (7 bytes)
     91   19       4       java.lang.AbstractStringBuilder::ensureCapacityInternal (27 bytes)
     92   20       3       java.lang.String::startsWith (7 bytes)
     92   21       3       java.lang.String::startsWith (72 bytes)
     92   22       3       java.lang.StringBuilder::append (8 bytes)
     93   26       4       java.lang.AbstractStringBuilder::append (29 bytes)
     93   24       3       java.lang.String::indexOf (70 bytes)
     93   25       3       java.lang.String::indexOf (166 bytes)
     94   23       3       java.io.WinNTFileSystem::isSlash (18 bytes)
     94   31       1       java.lang.Object::<init> (1 bytes)
     94    9       3       java.lang.Object::<init> (1 bytes)   made not entrant
     94   27  s    3       java.lang.StringBuffer::append (13 bytes)
     94   33     n 0       sun.misc.Unsafe::getObjectVolatile (native)   
     94   28       3       java.lang.Math::min (11 bytes)
     94   29       3       java.util.Arrays::copyOfRange (63 bytes)
[2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31]
     95   35       3       java.lang.AbstractStringBuilder::append (50 bytes)
```

- The first column : the number of the milliseconds since the VM started
- The second column : the order in which the method or code block was compiled(won't appear in order)
- The following columns may be empty, and there are lots of kinds of value :
  - n : native method
  - s : synchronized method
  - % : the code has been natively compiled and is running on a special part of memory called `code cache`
    - if it is shown, meaning the method is running in the most optimal way possible
- The next column ranges from 0 - 4 : meaning the degree of compilation
  - 0 : no compilation, so the code will be interpreted
  - 1 - 4 : different level of compilation has happened

### Compilation level

If we change the program argument from 10 to 5000, meaning we will have much more things to do :

```log
     81    1     n 0       java.lang.System::arraycopy (native)   (static)
     82    3       3       java.lang.String::hashCode (55 bytes)
     82    5       3       java.lang.String::equals (81 bytes)
     83    6       3       java.lang.String::getChars (62 bytes)
     83    7       3       java.io.WinNTFileSystem::normalize (143 bytes)
     83    8       4       java.lang.String::length (6 bytes)
     83    2       4       java.lang.String::charAt (29 bytes)
     83   11       3       java.lang.Object::<init> (1 bytes)
     83    9       3       java.lang.CharacterData::of (120 bytes)
     84   10       3       java.lang.CharacterDataLatin1::getProperties (11 bytes)
     84   13       3       java.lang.Character::toLowerCase (9 bytes)
     84   14       3       java.lang.CharacterDataLatin1::toLowerCase (39 bytes)
     84   15       3       java.lang.String::<init> (82 bytes)
     84   18       4       java.lang.AbstractStringBuilder::ensureCapacityInternal (27 bytes)
     84    4       3       java.lang.System::getSecurityManager (4 bytes)
     84   12       3       java.util.Arrays::copyOf (19 bytes)
     84   16       1       java.lang.ref.Reference::get (5 bytes)
     85   17       1       sun.instrument.TransformerManager::getSnapshotTransformerList (5 bytes)
     85   19       3       java.lang.String::startsWith (7 bytes)
     85   20       3       java.lang.String::startsWith (72 bytes)
     85   21       3       java.lang.StringBuilder::append (8 bytes)
     85   25       4       java.lang.AbstractStringBuilder::append (29 bytes)
     85   23       3       java.lang.String::indexOf (70 bytes)
     86   24       3       java.lang.String::indexOf (166 bytes)
     86   22       3       java.io.WinNTFileSystem::isSlash (18 bytes)
     86   26  s    3       java.lang.StringBuffer::append (13 bytes)
     86   27       3       java.lang.Math::min (11 bytes)
     86   30       1       java.lang.Object::<init> (1 bytes)
     87   11       3       java.lang.Object::<init> (1 bytes)   made not entrant
     87   29       3       java.lang.StringBuilder::append (8 bytes)
     87   28       3       java.util.Arrays::copyOfRange (63 bytes)
     87   31       3       java.lang.AbstractStringBuilder::append (50 bytes)
     87   33     n 0       sun.misc.Unsafe::getObjectVolatile (native)   
     87   34       1       java.lang.Integer::intValue (5 bytes)
     87   37       3       PrimeNumbers::isPrime (35 bytes)
     88   32       3       java.util.concurrent.ConcurrentHashMap::tabAt (21 bytes)
     88   41       3       java.lang.String::indexOf (7 bytes)
     88   35       1       java.util.ArrayList::size (5 bytes)
     88   40       3       java.lang.Integer::valueOf (32 bytes)
     88   38       3       java.lang.Number::<init> (5 bytes)
     88   42 %     4       PrimeNumbers::isPrime @ 2 (35 bytes)
     88   39       3       java.lang.Integer::<init> (10 bytes)
     88   36       1       java.lang.Boolean::booleanValue (5 bytes)
     89   43       3       java.util.ArrayList::add (29 bytes)
     89   44       3       java.util.ArrayList::ensureCapacityInternal (13 bytes)
     90   45       3       java.util.ArrayList::calculateCapacity (16 bytes)
     90   46       3       java.util.ArrayList::ensureExplicitCapacity (26 bytes)
     90   47       4       PrimeNumbers::isPrime (35 bytes)
     91   48       3       PrimeNumbers::getNextPrimeAbove (43 bytes)
     91   37       3       PrimeNumbers::isPrime (35 bytes)   made not entrant
    139   49       4       PrimeNumbers::getNextPrimeAbove (43 bytes)
    143   48       3       PrimeNumbers::getNextPrimeAbove (43 bytes)   made not entrant
```

We can see there appears a method is level 4 and tagged `%` , meaning that has been placed in the code cache.

There also appear several level 4 methods without tagged `%`