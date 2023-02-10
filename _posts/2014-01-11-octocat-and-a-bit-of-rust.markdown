---
layout: post
title: "Octocat and a bit of Rust"
date: 2014-01-11 03:17:13 +0700
comments: true
tags: Github Octocat Rust
---
After month of waiting[^1] I have finally received package from GitHub Shop with
a bunch of Octocat stickers. Stuck one on my notebook, one on my front door and
gave a few to a friends. Pretty cute little things. Definitely, Github has gained
so much popularity thanks to mascot.

![Octocats](/assets/images/octocats.jpeg)

Now for the code. As you may know, recently [Rust 0.9 had released](http://www.rust-lang.org/).
Today I was updating code in my small
[rosetta](https://github.com/JIghtuse/rosetta) repo (solutions for
[rosettacode](http://rosettacode.org/)) and discover one funny easter egg.

Here is a program for finding the limit of recursion in Rust 0.9 (version for
Rust 0.8 differs only by printing line).
```rust
fn recurse(n: int) {
    println!("deep: {:d}", n);
    recurse(n + 1);
}
 
fn main() {
    recurse(0);
}
```
This code has failing with segfault in Rust 0.8 printing last line `deep: 7892`.
Now it aborts on some assertion giving this great message:

>deep: 12952
>
>
>There are not many persons who know what wonders are opened to them in the
>stories and visions of their youth; for when as children we listen and dream,
>we think but half-formed thoughts, and when as men we try to remember, we are
>dulled and prosaic with the poison of life. But some of us awake in the night
>with strange phantasms of enchanted hills and gardens, of fountains that sing
>in the sun, of golden cliffs overhanging murmuring seas, of plains that stretch
>down to sleeping cities of bronze and stone, and of shadowy companies of heroes
>that ride caparisoned white horses along the edges of thick forests; and then
>we know that we have looked back through the ivory gates into that world of
>wonder which was ours before we were wise and unhappy.

>fatal runtime error:  assertion failed: !ptr.is_null()

>Aborted

Folks from [#rust on irc.mozilla.org](http://chat.mibbit.com/?server=irc.mozilla.org&channel=%23rust)
said that theoretically this code must print something like
`task '<main>' has overflowed its stack`, but overflow probably occurs in
println statement, so we have this nice Lovecraft quote instead.

[^1]: Russian Post especially cool on holidays.
