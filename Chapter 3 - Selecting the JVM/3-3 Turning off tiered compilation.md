# Turning off tiered compilation

`-XX:-TieredCompilation`

Force turing off the tier compilation

```
java -XX:-TieredCompilation -XX:+PrintCompilation Main 25000
```

As I experienced, the code running times are not significant difference between on and off.

But we can definitely see the difference of compilation process.

With flag

```log
     45    1             java.lang.StringLatin1::hashCode (42 bytes)
     66    2             java.lang.Integer::intValue (5 bytes)
     67    3             PrimeNumbers::isPrime (35 bytes)
     70    4             java.lang.Object::<init> (1 bytes)
     82    5             java.lang.Number::<init> (5 bytes)
     83    6             java.lang.Integer::<init> (10 bytes)
     84    7             java.lang.Integer::valueOf (32 bytes)
     85    8             java.lang.Boolean::booleanValue (5 bytes)
     85    9             PrimeNumbers::getNextPrimeAbove (43 bytes)
   1024   10             java.util.ArrayList::add (25 bytes)
   1026   11             java.util.ArrayList::add (23 bytes)
   1032   12             java.util.ArrayList::size (5 bytes)
   2180   13 %           PrimeNumbers::generateNumbers @ 30 (67 bytes)
   6607   13 %           PrimeNumbers::generateNumbers @ 30 (67 bytes)   made not entrant
Elapsed time was 6544 ms.
```

Without flag

```log
$ java -XX:+TieredCompilation -XX:+PrintCompilation Main 25000
     41    1       3       java.lang.StringLatin1::hashCode (42 bytes)
     43    2       3       java.lang.String::isLatin1 (19 bytes)
     44    5       3       java.lang.String::hashCode (49 bytes)
     45    9       3       java.util.ImmutableCollections$SetN::probe (56 bytes)
     46   12       4       java.lang.StringLatin1::hashCode (42 bytes)
     47   11       3       java.lang.StringLatin1::equals (36 bytes)
     48   13       3       java.util.ImmutableCollections$SetN::hashCode (46 bytes)
     48    4       3       java.lang.Object::<init> (1 bytes)
     49    1       3       java.lang.StringLatin1::hashCode (42 bytes)   made not entrant
     50    6       3       java.lang.String::coder (15 bytes)
     50   15       3       java.util.Objects::requireNonNull (14 bytes)
     51   14       3       java.util.ImmutableCollections::emptySet (4 bytes)
     52   18       3       java.lang.StringLatin1::charAt (28 bytes)
     53    7       3       java.lang.Math::floorMod (10 bytes)
     53   22       4       java.lang.Object::<init> (1 bytes)
     54   29     n 0       jdk.internal.misc.Unsafe::getObjectVolatile (native)
     55    8       3       java.lang.Math::floorDiv (22 bytes)
     56    4       3       java.lang.Object::<init> (1 bytes)   made not entrant
     56   26       3       java.util.ImmutableCollections$SetN$SetNIterator::next (47 bytes)
     57   33     n 0       jdk.internal.misc.Unsafe::compareAndSetLong (native)
     58   19       3       java.util.ImmutableCollections$SetN$SetNIterator::hasNext (13 bytes)
     59   20       3       java.util.ImmutableCollections$SetN$SetNIterator::nextIndex (56 bytes)
     59   34   !   3       java.util.concurrent.ConcurrentHashMap::putVal (432 bytes)
     62   37       3       java.util.HashMap::hash (20 bytes)
     62   43       4       java.lang.String::hashCode (49 bytes)
     63   44       3       java.util.HashMap::putVal (300 bytes)
     65   45       3       java.util.HashMap$Node::<init> (26 bytes)
     65   46       3       java.util.HashMap::newNode (13 bytes)
     66    5       3       java.lang.String::hashCode (49 bytes)   made not entrant
     67   48       4       java.util.ImmutableCollections$SetN$SetNIterator::hasNext (13 bytes)
     68   47       3       java.util.HashMap::afterNodeInsertion (1 bytes)
     69   19       3       java.util.ImmutableCollections$SetN$SetNIterator::hasNext (13 bytes)   made not entrant
     69   17       1       java.lang.module.ModuleReference::descriptor (5 bytes)
     70    3       3       java.lang.String::charAt (25 bytes)
     71   16       1       java.lang.module.ModuleDescriptor::name (5 bytes)
     72   25       1       java.lang.module.ResolvedModule::reference (5 bytes)
     72   39       1       java.lang.module.ModuleDescriptor$Exports::source (5 bytes)
     73   43       4       java.lang.String::hashCode (49 bytes)   made not entrant
     74   24       3       java.util.Objects::equals (23 bytes)
     74   35       1       java.util.ImmutableCollections$SetN::size (5 bytes)
     75   52       3       java.lang.String::hashCode (49 bytes)
     76   55       3       PrimeNumbers::isPrime (35 bytes)
     77   36       3       java.lang.String::length (11 bytes)
     78   57 %     4       PrimeNumbers::isPrime @ 2 (35 bytes)
     78   54       1       java.lang.Boolean::booleanValue (5 bytes)
     79   58       3       java.lang.Number::<init> (5 bytes)
     80   59       3       java.lang.Integer::<init> (10 bytes)
     80   61       4       PrimeNumbers::isPrime (35 bytes)
     81   60       3       java.lang.Integer::valueOf (32 bytes)
     82   53       1       java.lang.Integer::intValue (5 bytes)
     83   55       3       PrimeNumbers::isPrime (35 bytes)   made not entrant
     84   62       3       java.lang.Boolean::valueOf (14 bytes)
     85   32       3       java.util.concurrent.ConcurrentHashMap::addCount (289 bytes)
     86   56       1       java.util.ArrayList::size (5 bytes)
     86   64       4       java.lang.Integer::valueOf (32 bytes)
     87   27       3       java.util.concurrent.ConcurrentHashMap::tabAt (22 bytes)
     88   60       3       java.lang.Integer::valueOf (32 bytes)   made not entrant
     89   65       3       PrimeNumbers::getNextPrimeAbove (43 bytes)
     89   66       3       java.util.ArrayList::add (23 bytes)
     90   63       3       java.util.ArrayList::add (25 bytes)
     91   28       3       jdk.internal.misc.Unsafe::getObjectAcquire (7 bytes)
     92   10       3       java.lang.String::equals (65 bytes)
     93   30       3       java.util.concurrent.ConcurrentHashMap::spread (10 bytes)
     93   31       3       java.util.concurrent.ConcurrentHashMap$Node::<init> (20 bytes)
     94   51       3       java.util.concurrent.ConcurrentHashMap::putIfAbsent (8 bytes)
     95   42       3       java.util.HashMap::put (13 bytes)
     96   21       1       java.util.KeyValueHolder::getKey (5 bytes)
     96   23       1       java.util.KeyValueHolder::getValue (5 bytes)
     97   38       1       java.lang.module.ModuleDescriptor::isAutomatic (5 bytes)
     98   49       1       java.lang.module.ModuleDescriptor::isOpen (5 bytes)
     99   40       1       java.lang.module.ResolvedModule::configuration (5 bytes)
     99   50       1       java.lang.Module::getDescriptor (5 bytes)
    100   41       1       java.lang.module.ModuleDescriptor$Exports::targets (5 bytes)
    131   67       4       PrimeNumbers::getNextPrimeAbove (43 bytes)
    137   65       3       PrimeNumbers::getNextPrimeAbove (43 bytes)   made not entrant
    363   68       4       java.util.ArrayList::add (25 bytes)
    366   63       3       java.util.ArrayList::add (25 bytes)   made not entrant
   6637   69     n 0       java.lang.System::arraycopy (native)   (static)
Elapsed time was 6562 ms.
```