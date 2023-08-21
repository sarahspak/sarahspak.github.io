---
layout: post
title:  "Ruby and openssl"
date:   2023-08-21 09:49:08 -0400
categories: openssl ruby ldflags
---
## ruby somehow used the wrong version of openssl...
Tried to install `Ruby 3.1.3` for Jekyll. Got `chruby` and a version of `ruby` installed fairly easily. 
Howeer, I kept failing to install `ruby 3.1.3` (version suggested by the jekyll [macosx guide](https://jekyllrb.com/docs/installation/macos/)) but it was impossible - kept getting some weird ssl error about a variable being unset. Turns out it was because ruby was using openssl@3 (which, btw, I never explicitly installed - homebrew installed it because apparently one of my other applications managed by homebrew requires 3, not 1.1). This meant I needed to declare some env variables before running `ruby-install ruby 3.1.3`. The specific incantation was as follows: 

{% highlight bash%}
# install ruby using ruby-install and openssl@1.1
export PKG_CONFIG_PATH="/opt/homebrew/opt/openssl@1.1/lib/pkgconfig"
export RUBY_CONFIGURE_OPTS="--with-openssl-dir=/opt/homebrew/opt/openssl@1.1"
export LDFLAGS="-L/opt/homebrew/opt/openssl@1.1/lib"
export CPPFLAGS="-I/opt/homebrew/opt/openssl@1.1/include"
ruby-install ruby 3.1.3
{% endhighlight %}

After I was able to get this website live with jekyll/resolve the issue, I wanted to answer a few questions:
- what are LDFLAGS?  
- why is openssl so ubiquitious? what purpose does it serve?
- why would you ever want to use ruby?

And a few random questions:
- can I use obsidian to create these blog posts? (the answer is a resounding yes)


## What are LDFLAGS?
From ChatGippity, with some paraphrasing by me:  

LDFLAGS is an environment variable commonly used in Unix-like operating systems (hello, Mac) during the software build process. It specifies flags that should be passed to the linker (ld).  
The linker is the tool that takes one or more object files (usually produced by a compiler) and combines them into a single executable or a library.  
Here's a breakdown of what LDFLAGS does (all of these are in makefiles):  
1. Library Paths: If you have libraries installed in non-standard locations, you can use LDFLAGS to tell the linker where to find them. For example:  
`LDFLAGS="-L/opt/somelib/lib"`  
The -L flag tells the linker to add the specified directory to its list of directories to search for libraries.

2. Link Libraries: While -l (lowercase L) is commonly used in the compile command to specify a library to link against, it can also be part of LDFLAGS. For example:

`LDFLAGS="-lm"`  

This tells the linker to link against the math library (libm.so or libm.a).

3. Other Linker Options: There are many other options you can pass to the linker, and LDFLAGS is where you'd specify them. For instance, you might set options to control the optimization of the resulting binary, to generate map files, or to control how undefined symbols are handled.

4. Shared Libraries and Dynamic Linking: When building shared libraries or dynamically linked applications, specific flags might be required to tell the linker to produce the correct type of output. These flags would be specified in LDFLAGS.

In many build systems, especially those based on make, LDFLAGS is a standard variable that the build system will automatically pass to the linker when building the software. By setting or modifying LDFLAGS, you can control the linking process without having to modify the build system's configuration or makefiles.

In summary, LDFLAGS is an environment variable that provides flags to the linker during the software build process, allowing developers to specify where to find libraries, which libraries to link against, and other linker-specific options.









