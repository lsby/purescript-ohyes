# Purescript-OhYes

```
in  upstream
  with ohyes =
      { dependencies =
            [ "aff"
            , "effect"
            , "foldable-traversable"
            , "functions"
            , "has-js-rep"
            , "lists"
            , "node-buffer"
            , "node-fs"
            , "nullable"
            , "prelude"
            , "prettier"
            , "psci-support"
            , "spec"
            , "tuples"
            , "typelevel-prelude"
            , "variant"
            ]
      , repo =
          "https://github.com/lsby/purescript-ohyes"
      , version =
          "ls-v1.0.2"
      }
  with has-js-rep =
      { dependencies =
            [ "aff-promise"
            , "arrays"
            , "console"
            , "effect"
            , "foldable-traversable"
            , "foreign-object"
            , "functions"
            , "nullable"
            , "prelude"
            , "psci-support"
            , "record-format"
            , "strings"
            , "tuples"
            , "typelevel-prelude"
            , "variant"
            ]
      , repo =
          "https://github.com/lsby/purescript-has-js-rep"
      , version =
          "ls-v1.0.1"
      }
  with record-format =
      { dependencies =
            [ "assert"
            , "effect"
            , "prelude"
            , "psci-support"
            , "record"
            , "typelevel-prelude"
            ]
      , repo =
          "https://github.com/lsby/purescript-record-format"
      , version =
          "ls-v1.0.1"
      }
```

```
spago install ohyes
npm i prettier
```

代码示例:

```
module Main where

import Prelude

import Effect (Effect)
import Data.Foldable (intercalate)
import Data.Function.Uncurried (Fn2)
import Data.Nullable (Nullable)
import Data.Variant (Variant)
import Node.Encoding (Encoding(..))
import Node.FS.Sync (writeTextFile)
import OhYes (generateTS)
import Text.Prettier (defaultOptions, format)
import Type.Proxy (Proxy(..))

type A =
  { a :: Number
  , b :: String
  , c :: { d :: String }
  , e :: Array String
  , f :: Nullable String
  , g :: Number -> Number -> Number
  , h :: Fn2 Number Number Number
  , i :: Fn2 Number (Fn2 Number Number Number) Number
  }

type VariantTest = Variant
  ( a :: String
  , b :: Number
  , c :: Boolean
  )


main :: Effect Unit
main = writeTextFile UTF8 "./test/generated.ts" values
  where
    values = format defaultOptions $ intercalate "\n"
      [ generateTS "A" (Proxy :: Proxy A)
      , generateTS "VariantTest" (Proxy :: Proxy VariantTest)
      ]
```

自定义类型的实现:

```
module Main where

import Prelude
import Data.Foldable (intercalate)
import Effect (Effect)
import HasJSRep (class HasJSRep)
import Node.Encoding (Encoding(..))
import Node.FS.Sync (writeTextFile)
import OhYes (class HasTSRep, generateTS)
import Text.Prettier (defaultOptions, format)
import Type.Proxy (Proxy(..))

data AAA

instance dfgdfg :: HasJSRep AAA

instance sdgsd :: HasTSRep AAA where
  toTSRep _ = "string"

main :: Effect Unit
main = writeTextFile UTF8 "./test/generated.ts" values
  where
  values =
    format defaultOptions
      $ intercalate "\n"
          [ generateTS "AAA" (Proxy :: Proxy AAA)
          ]

```
