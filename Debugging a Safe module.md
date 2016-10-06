# Debugging a Safe module

TL;DR: change `-XSafe` to `-XTrustworthy`.

I've found myself needing to add some trace statements to `System/Environment.hs`.
The module is marked `Safe`, so when you try to:
```
import Debug.Trace (trace)
```

you get the following error:
```
Debug.Trace: Can't be safely imported!
The module itself isn't safe.
```

I don't know anything about Safe Haskell and I didn't want to learn at the time. 
Removing `{-# LANGUAGE Safe #-}` made the `Safe` modules depending on `System.Environment` fail to compile, so that wasn't an option.
Turns out you can just bypass this by marking your module as `Trustworthy`:

```
{-# LANGUAGE Trustworthy #-}
```

Well, TIL.
