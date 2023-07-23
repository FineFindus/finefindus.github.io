+++
title = "artem 2.0.0"
description = "Summary of the changes in the new v2.0.0 artem release"
date = 2023-07-23
[taxonomies]
categories = ["rust"]
+++

# artem 2.0.0

A new version of [`artem`](https://github.com/FineFindus/artem) has been released. artem is an image-to-ASCII converter.
This release brings various performance and code improvements, as well as full Unicode support, which makes images like this possible.

![ASCII art of an emoji, consisting of emoji](/posts/artem_2_0/unicode_example.png)

Overall, artem should now be 10-16% faster.

## CLI

The `-h` flag has been changed to show the help information instead of resizing the image to the terminal height.
To use the terminal height, the `--height` flag is still available. Due to an upgrade of the underlying CLI crate, the help output should now also be more readable.

As mentioned earlier, it is now possible to use artem with Unicode characters using `--characters "🙂"` or just `-c "🙃"` for short. 
Note that the usual rules for using a custom character set apply, see the man page or use the `--help` command to learn more. In addition, many Unicode characters
such as emoticons, have a different size than standard ASCII characters, so using both can cause distortion.

## Using `artem` as a crate

If you've ever tried to use `artem` version 1 as a library and looked at its [docs.rs page](https://docs.rs/artem/1.2.1/artem/index.html), you may have noticed a warning,
stating that artem should not be used as a library. This did not stop people from using it, so version 2 provides more explicit support.
It now provides sample code for converting an image, and the `Option` which is used to configure the conversion has been renamed to `Config` so that it no longer conflicts with the 
with rust's built-in `Option`.

Previously `convert()` took the configuration by value, so to convert multiple images with the same configuration it was necessary to either clone the config or rebuild it. 
rebuild it each time. Now it just takes a reference to the `config`.

#### Migration to V2

If you're already use artem, all you have to do is replace `OptionBuilder` with `ConfigBuilder` and pass it as a reference to `convert()`.

## Future 

As `-h` is commonly used as a shorthand for help, the --height function currently lacks a short alias.

A lot of time is currently spent on colouring the output. artem uses [`coloured`](https://crates.io/crates/colored), which checks each time it is invoked
whether the string should be coloured. In artems case this is every single character that is converted/printed, which adds up. As only small part of the whole feature set is used,
it should be possible to include it directly in artem.
