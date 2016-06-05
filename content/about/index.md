+++
date = "2016-06-03T14:52:23+02:00"
description = ""
title = "About"
hidden = "true"
nav = "true"

+++

Hello, My name is Robert den Harink. I'm a software developer from the Netherlands. <br /> I keep this blog to remember things and share interesting rubbish for your entertainment.

I like functional programming, Linux, graphic design, creating funky music with Ableton/vintage-organs and traveling in my old 'krankenwagen' — that's german for ambulance.

Currently i work for a company that builds software for the Dutch government. I take care of architecture
and security. I'm also available as freelancer

***

If you have questions/comments or you just want to say hi, drop me a message and i'll get back at you.

To generate my contact information please run one of the scripts below

__Haskell__

```haskell
-- Generate contact information for Robert den Harink
-- http://www.robhar.com

import Data.Char (ord, chr)

crypt :: (Char -> Char -> Int) -> String -> String -> String
crypt f key = map toLetter . zipWith f (cycle key)
    where toLetter = chr . (+) (ord '.')

enc :: Char -> Char -> Int
enc k c = (base k + base c) `mod` 77
    where base c = (ord c) - (ord '.')

dec :: Char -> Char -> Int
dec k c = (base c - base k) `mod` 77
    where base c = (ord c) - (ord '.')

encrypt = crypt enc
decrypt = crypt dec

main :: IO()
main = putStrLn $ decrypt "secret" "jYJ\\\\m8\\WYRZjeKfWuumjtmx.imt"
```

__Go__
```golang
// Generate contact information for Robert den Harink
// http://www.robhar.com
package main

import "fmt"

const (
    start = '.'
    end   = 'z'
)

type key string

func main() {
    k := key("secret")
    decrypted := k.decrypt([]byte("kZK]]n9]XZS[keLgXuumjtmx/imt"))
    fmt.Println(string(decrypted))
}

func (k key) encrypt(tx []byte) []byte {
    ct := tx
    for i, c := range ct {
            ct[i] = start + (c-start+k[i%len(k)]-start)%(end-start)
    }

    return ct
}

func (k key) decrypt(tx []byte) []byte {
    pt := tx
    for i, c := range pt {
            pt[i] = start + (c-k[i%len(k)]+(end-start))%(end-start)
    }

    return pt
}
```