---
title: CppCheck
description: Source code analyser
author: flatcap
---

# {{ page.title }}

{:.subtitle}
{{ page.description }}
{% include analysis-links.html %}

- [http://cppcheck.sourceforge.net](http://cppcheck.sourceforge.net)

Since NeoMutt's code base is relatively big, it is easier to execute CppCheck from the command line and let it save its analysis to an output file.

CppCheck is available on all major platforms, so it can probably be installed through the package manager of your system.
In case you are using Windows (but not Cygwin) it is possible to download the installer from the website.
We can use `cppcheck-gui` afterwards for filtering the warnings we want to analyze.

You can use following command for analyzing the source code

```sh
cppcheck --enable=all --language=c --std=c99 -i autosetup --platform=unspecified -D_POSIX_PATH_MAX=4096 -DPATH_MAX=2048 -I/usr/include -I/usr/include/mit-krb5 -I. --output-file=cppcheck.xml --xml --xml-version=2 <path to neomutt source code directory>
```

A complete description of all parameters can be found through `man cppcheck` or `cppcheck --help`.
Some considerations:

- `--enable-all --output-file=<filename> --xml --xml-version=2` collect as many warning as possible and save them to a file, in order to view them later with `cppcheck-gui`
- `--platform=unspecified` do not restrict the analysis to a 32 or 64 bit platform
- `--language=c --std=c11` NeoMutt follows this standard
- `-D_POSIX_PATH_MAX=4096 -DPATH_MAX=2048` can be left out, but CppCheck may warn that it does not know the values for those macros. Since most of the time the actual values are not relevant, you might just set them to two numeric values.
- `-i autosetup` for excluding the `autosetup` directory
- `-I<path>` where to find include files

If cppcheck takes too much time for analyzing the source code, it is always possible to analyze only specific files, tell it to test less macro combinations, or to not enable all warnings.

