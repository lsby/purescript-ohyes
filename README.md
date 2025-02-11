# Purescript-OhYes

先添加[上游](https://github.com/lsby/purescript-my-upstream)

安装:
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

instance HasJSRep AAA
instance HasTSRep AAA where
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
