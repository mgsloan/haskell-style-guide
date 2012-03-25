MGSloan Haskell Style
===================

This is a short document describing my preferred Haskell style, which
is a somewhat compactified version of the style outlined in tibbe's
haskell-style-guide.

Formatting
----------

### Line Length

Maximum line length is *80 characters*.

### Indentation

Tabs are illegal. Use a minimal number spaces for indenting, while
still having consistent indentation.  This ends up being giving
everything *2 space* indents, with `where` indented by one.

```haskell
sayHello :: IO ()
sayHello = do
  name <- getLine
  putStrLn $ greeting name
 where
  greeting name = "Hello, " ++ name ++ "!"

filter :: (a -> Bool) -> [a] -> [a]
filter _ []   = []
filter p (x:xs)
  | p x       = x : filter p xs
  | otherwise = filter p xs
```

When definitions get longer than one line, and the root of the expression
is not a short `do` / `let` / `if` / `case` introduction, place the function
definition "=" symbol on the next line.

TODO: Example of this


```haskell

### Blank Lines

One blank line between top-level definitions.     No blank lines between
type signatures and function definitions.  Add one blank line between
functions in a type class instance declaration if the functions bodies
are large.  Use your judgement.

### Whitespace

Surround binary operators with a single space on either side.  Use
your better judgement for the insertion of spaces around arithmetic
operators but always be consistent about whitespace on either side of
a binary operator.  Don't insert a space after a lambda.  Pad
multiline parenthesis with a space.

### Data Declarations

Align the constructors in a data type definition.  Example:

```haskell
data Tree a = Branch a (Tree a) (Tree a)
            | Leaf
```

For long type names the following formatting is also acceptable:

```haskell
data HttpException
  = InvalidStatusCode Int
  | MissingContentHeader
```

Format records as follows:

```haskell
data Person = Person
  { firstName :: String  -- ^ First name
  , lastName  :: String  -- ^ Last name
  , age       :: Int     -- ^ Age
  } deriving (Eq, Show)
```

### Serial Declarations

Align the elements of lists, tuples, and curly brace/semicolon syntax.  Example:

```haskell
exceptions =
  [ InvalidStatusCode
  , MissingContentHeader
  , InternalServerError
  ]
```

Optionally, you can skip the first newline.  Use your judgement.

```haskell
directions = [ North
             , East
             , South
             , West
             ]
```

TODO: Extend this to boolean operators (particularly ($) and (.))


### Pragmas

Put pragmas immediately following the function they apply to.
Example:

```haskell
id :: a -> a
id x = x
{-# INLINE id #-}
```

In the case of data type definitions you must put the pragma before
the type it applies to.  Example:

```haskell
data Array e = Array
    {-# UNPACK #-} !Int
    !ByteArray
```

### Hanging Lambdas

You may or may not indent the code following a "hanging" lambda.  Use
your judgement. Some examples:

```haskell
bar :: IO ()
bar = forM_ [1, 2, 3] $ \n -> do
          putStrLn "Here comes a number!"
          print n

foo :: IO ()
foo = alloca 10 $ \a ->
      alloca 20 $ \b ->
      cFunction a b
```

### Export Lists

Format export lists as follows:

```haskell
module Data.Set
  ( 
  -- * The @Set@ type
    Set
  , empty
  , singleton

  -- * Querying
  , member
  ) where
```

Imports
-------

Imports should be grouped in the following order:

1. standard library imports
2. related third party imports
3. local application/library specific imports

Put a blank line between each group of imports.  The imports in each
group should be sorted alphabetically, by module name.

Always use explicit import lists or `qualified` imports for standard
and third party libraries.  This makes the code more robust against
changes in these libraries.  Exception: The Prelude.

Comments
--------

### Punctuation

Write proper sentences; start with a capital letter and use proper
punctuation.

### Top-Level Definitions

Comment every top level function (particularly exported functions),
and provide a type signature; use Haddock syntax in the comments.
Comment every exported data type.  Some examples:

```haskell
-- | Send a message on a socket.  The socket must be in a connected
-- state.  Returns the number of bytes sent.  Applications are
-- responsible for ensuring that all data has been sent.
send :: Socket      -- ^ Connected socket
     -> ByteString  -- ^ Data to send
     -> IO Int      -- ^ Bytes sent

-- | Bla bla bla.
data Person = Person
    { age  :: Int     -- ^ Age
    , name :: String  -- ^ First name
    }
```

For functions the documentation should give enough information to
apply the function without looking at the function's definition.

### End-of-Line Comments

Separate end-of-line comments from the code using 2 spaces.  Align
comments for data type definitions.  Some examples:

```haskell
data Parser = Parser
  Int         -- Current position
  ByteString  -- Remaining input

foo :: Int -> Int
foo n = salt * 32 + 9
 where
  salt = 453645243  -- Magic hash salt.
```

### Links

Use in-line links economically.  You are encouraged to add links for
API names.  It is not necessary to add links for all API names in a
Haddock comment.  We therefore recommend adding a link to an API name
if:

* The user might actually want to click on it for more information (in
  your judgment), and

* Only for the first occurrence of each API name in the comment (don't
  bother repeating a link)

Naming
------

Use mixed-case when naming functions and camel-case when naming data
types.

For readability reasons, don't capitalize all letters when using an
abbreviation.  For example, write `HttpServer` instead of
`HTTPServer`.  Exception: Two letter abbreviations, e.g. `IO`.

### Modules

Use singular when naming modules e.g. use `Data.Map` and
`Data.ByteString.Internal` instead of `Data.Maps` and
`Data.ByteString.Internals`.

Misc
----

### Warnings ###

Code should be compilable with `-Wall -Werror`. There should be no
warnings.
