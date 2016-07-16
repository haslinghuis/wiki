# Known  Build Environment Problems
## Cygwin on Windows
ccache 3.1.9 in Cygwin 2.874 is broken. Returns false cache hits. Do not install. Betaflight build system uses ccache if available. 