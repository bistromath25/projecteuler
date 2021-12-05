# Project Euler 187 - Semiprimes

[Problem statement](https://projecteuler.net/problem=187)

It's possible to find all primes numbers less than 100'000'000 / 2 = 50'000'000 and then increment our answer for only those whose products are less than 1'00'000'000. We can find all primes under a certain N by using a Sieve of Eratosthenes, which determines all primes under N in O(Nlog(log(N))). Below is a prime sieve implementation. The function `eratosthenes_helper` returns a `vector<bool>` which is passed into `eratosthenes` which returns a `vector<int>` containing primes not greater than `n`.

```c++
inline vector<bool> eratosthenes_helper(int n) {
    vector<bool> primes(n + 1, true);
    primes[0] = false;
    primes[1] = false;
    if (n >= 4) {
        for (int j = 2 * 2; j <= n; j += 2)
            primes[j] = false;
    }
    int r = int(sqrt(n));
    for (int i = 3; i <= r; i += 2) {
        if (primes[i]) {
            for (int j = i * i; j <= n; j += i)
                primes[j] = false;
        }
    }
    return primes;
}

inline vector<int> eratosthenes(int n) {
    vector<bool> p = eratosthenes_helper(n);
    vector<int> primes;
    if (n >= 2) primes.push_back(2);
    for (int i = 3; i <= n; i += 2)
        if (p[i]) primes.push_back(i);
    return primes;
}
```

We'll use the above sieve to determine all primes under 50'000'000, and then iterate through our prime list, incrementing our answer each time the product is less than 100'000'000. 

```c++
int ans = 0;
for (int i = 0; primes[i] * primes[i] < 100000000; ++i) {
    for (int j = i; primes[i] * primes[j] < 100000000; ++j) {
        ans++;
    }
}
cout << ans << '\n';
```

Thanks for reading, see you in the next one :) 
