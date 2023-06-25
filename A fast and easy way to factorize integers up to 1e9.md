[Original link](https://codeforces.com/blog/entry/117622)

# [Tutorial] A fast and easy way to factorize integers up to 1e9

This blog focuses on the GCC compiler.

The most basic way to find a prime factorization for an integer $n$ is to test every possible divisor up to $\sqrt n$. This can be optimized by precomputing primes and only testing those. What else can we do? Note that the modulo operation is rather expensive. To optimize it, we can use something like the Montgomery multiplication, but it's too complicated for an "easy way". But also note that when the divisor is a compile-time constant, the compiler optimizes the modulo operation (or division, for that matter) to cheap multiplication and bit-shift operations. Let's take advantage of that:

```c++
#include <bits/stdc++.h>
using namespace std;

typedef uint32_t u32;

// The cold attribute tells the compiler that this function is unlikely to be
// called.  Without it, compilation will take much more time because the
// compiler tries to optimize the function calls. Attributes are a GCC
// extension.
__attribute__((cold))
void factor_helper(vector<u32> &vec, u32 &x, u32 y)
{
    do {
        vec.push_back(y);
        x /= y;
    } while (x % y == 0);
}

vector<u32> factor(u32 x) {
    vector<u32> vec;
#define F(y) if (x%y == 0) { factor_helper(vec, x, y); }
    F(2)F(3)F(5)F(7)F(11)F(13)... /* all primes up to sqrt(1e9) */
#undef F
    if (x > 1)
        vec.push_back(x);
    return vec;
}
```

As you can see, it's pretty simple and the long line containing all primes can be generated with a few lines of code. The only problem that remains is its long compile time. The above code is tweaked in a way so that the compilation can be done within a few seconds with GCC (which has been achieved through trial and error, like how the parameters are passed, the cold attribute, etc.), so that's no longer an issue either. Also unsigned integers are used because they are faster in this case.

But how fast actually is this? With my tests it can factorize $10^6$ integers in less than 2 seconds! Feel free to test it for yourself (Be sure to use the latest version of g++ available on Codeforces for testing or actually using this).
