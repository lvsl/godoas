# Dev Journal

## Relevant readings

### Set UID
Setuid programs have a long history and generally considered badly designed.

Couple of articles referenced on wikipedia that go into some details of the issue, a bit outdated:
* [Setuid Demystified](http://www.cs.berkeley.edu/~daw/papers/setuid-usenix02.pdf)
* [Revising Setuid Demystified](http://www.eecs.berkeley.edu/~daw/papers/setuid-login08b.pdf)

Main points are:
* (uid, euid, suid) - three identities available for manipulation
* Mind Linux's `CAP_SETUID` capabilities
* Aux groups inherited work differently
* `setuid` is not cross platform, use [setresuid(2)](http://man7.org/linux/man-pages/man2/setresuid.2.html)
* On Linux (uid, euid, suid) are per-thread
* Linux has [fsuid](http://man7.org/linux/man-pages/man2/setfsuid.2.html), badly designed feature
* GCC implementation of `setuid` is [crazy](https://github.com/bminor/glibc/blob/master/nptl/allocatestack.c#L1050)
* Musl implementation is [better](https://github.com/bminor/musl/blob/master/src/unistd/setxid.c#L12)

### Linux Capabilities

Michael Kerrisk's [presentation](http://man7.org/conf/osseu2019/Linux-capabilities-model-OSS.eu-2019--Kerrisk.pdf) is a good start.

Some key points:
* Beware of `CAP_SYS_ADMIN`
* Capabilities are per-thread
* Stored in extended attrs of filesystem
* Rules of behavior after fork/exec are different

### Seccomp

Michael Kerrisk's [article](http://man7.org/conf/meetup/seccomp--jambit-Kerrisk-2019-05-08.pdf) is a good start.


## MVP Plan
* Golang 1.13 and Linux 5.2. Not cross platform.
* Use `CAP_SETUID` instead of set uid bit. Would this work re. fork/exec?
* Port tests from OpenBSD doas
* Think about cgroups/ttys/limits after fork/exec
* OpenBSD does uses pledge/unveil. In linux it's seccomp and what for unveil (look at Chrome's [minijail](https://android.googlesource.com/platform/external/minijail/))?
* Must be static binary so can't set LD_PRELOAD_PATH, etc.
* The setuid issue for Go was investigated in https://github.com/golang/go/issues/1435
* Use [runtime.LockOSThread](https://golang.org/pkg/runtime/#LockOSThread) in init to make sure main goroutine is on a single thread.
