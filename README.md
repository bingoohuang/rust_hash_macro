# Rust代码安全：混淆的魔力

[文章](https://mp.weixin.qq.com/s/Fk9on6PGht_CkPG6ohiSIQ) 代码部分


```sh
$ cargo build --release
   Compiling hash_macro v0.1.0 (/Users/bingoo/RustroverProjects/has_macro)
    Finished release [optimized] target(s) in 1.13s
```

```sh
$ target/release/hash_macro SUPER_SECRET_PASSWORD
Correct KEY: 0a96c5c0592af89c976b86b8caadb58cd3e7716224896da2ca3c8dff7c799f37
$ target/release/hash_macro HHHH                 
Incorrect
```

```sh
$ cargo expand
#![feature(prelude_import)]
#[prelude_import]
use std::prelude::rust_2021::*;
#[macro_use]
extern crate std;
use sha2::{Digest, Sha256};
use std::env;
const KEY: &str = "SUPER_XXX";
fn main() {
    let guess = env::args().nth(1).expect("No guess given");
    if KEY == guess {
        {
            ::std::io::_print(format_args!("Correct1\n"));
        };
    } else {
        {
            ::std::io::_print(format_args!("Incorrect1\n"));
        };
    }
    let key = {
        let input = {
            let res = ::alloc::fmt::format(format_args!("{0}{1}", "SUPER_YYY", "SALT"));
            res
        };
        let mut hasher = Sha256::new();
        hasher.update(input);
        hasher.finalize()
    };
    let guess_hash = {
        let input = {
            let res = ::alloc::fmt::format(format_args!("{0}{1}", guess, "SALT"));
            res
        };
        let mut hasher = Sha256::new();
        hasher.update(input);
        hasher.finalize()
    };
    if key == guess_hash {
        {
            ::std::io::_print(format_args!("Correct2, KEY: {0:x}\n", key));
        };
    } else {
        {
            ::std::io::_print(format_args!("Incorrect2\n"));
        };
    }
}

```