### `Sieve of Eratosthenes:`

Sieve of Eratosthenes is an algorithm for finding all the prime numbers in a segment ![](https://latex.codecogs.com/svg.latex?%5B1%3B%20n%5D) using ![](https://latex.codecogs.com/svg.latex?O%28n%20%5Clog%20%5Clog%20n%29) operations.

The algorithm is very simple. At the beginning we write down all numbers between 2 and ![](https://latex.codecogs.com/svg.latex?n). We mark all proper multiples of 2 (since 2 is the smallest prime number) as composite.
A proper multiple of a number ![](https://latex.codecogs.com/svg.latex?x), is a number greater than ![](https://latex.codecogs.com/svg.latex?x) and divisible by ![](https://latex.codecogs.com/svg.latex?x).
Then we find the next number that hasn't been marked as composite, in this case it is 3.
Which means 3 is prime, and we mark all proper multiples of 3 as composite.
The next unmarked number is 5, which is the next prime number, and we mark all proper multiples of it.
And we continue this procedure until we processed all numbers in the row.

In the following image you can see a visualization of the algorithm for computing all prime numbers in the range ![](https://latex.codecogs.com/svg.latex?%5B1%3B%2016%5D). It can be seen, that quite often we mark numbers as composite multiple times.

![Sieve of Eratosthenes](https://github.com/aam-oj-cp/Work-Space/blob/main/pictures/sieve_eratosthenes.png)

The idea behind is this:
A number is prime, if none of the smaller prime numbers divides it.
Since we iterate over the prime numbers in order, we already marked all numbers, who are divisible by at least one of the prime numbers, as divisible.
Hence if we reach a cell and it is not marked, then it isn't divisible by any smaller prime number and therefore has to be prime.

### `Implementation:`

```cpp
int n;
vector<char> is_prime(n+1, true);
is_prime[0] = is_prime[1] = false;
for (int i = 2; i <= n; i++) {
    if (is_prime[i] && (long long)i * i <= n) {
        for (int j = i * i; j <= n; j += i)
            is_prime[j] = false;
    }
}
```

This code first marks all numbers except zero and one as potential prime numbers, then it begins the process of sifting composite numbers.
For this it iterates over all numbers from ![](https://latex.codecogs.com/svg.latex?2) to ![](https://latex.codecogs.com/svg.latex?n).
If the current number ![](https://latex.codecogs.com/svg.latex?i) is a prime number, it marks all numbers that are multiples of ![](https://latex.codecogs.com/svg.latex?i) as composite numbers, starting from ![](https://latex.codecogs.com/svg.latex?i%5E2).
This is already an optimization over naive way of implementing it, and is allowed as all smaller numbers that are multiples of ![](https://latex.codecogs.com/svg.latex?i) necessary also have a prime factor which is less than ![](https://latex.codecogs.com/svg.latex?i), so all of them were already sifted earlier.
Since ![](https://latex.codecogs.com/svg.latex?i%5E2) can easily overflow the type `int`, the additional verification is done using type `long long` before the second nested loop.

Using such implementation the algorithm consumes ![](https://latex.codecogs.com/svg.latex?O%28n%29) of the memory (obviously) and performs ![](https://latex.codecogs.com/svg.latex?O%28n%20%5Clog%20%5Clog%20n%29) (see next section).

### `Asymptotic analysis:`

Let's prove that algorithm's running time is ![](https://latex.codecogs.com/svg.latex?O%28n%20%5Clog%20%5Clog%20n%29).
The algorithm will perform ![](https://latex.codecogs.com/svg.latex?%5Ctiny%20%5Cfrac%7Bn%7D%7Bp%7D) operations for every prime ![](https://latex.codecogs.com/svg.latex?p%20%5Cle%20n) the inner loop.
Hence, we need to evaluate the next expression:

![](https://latex.codecogs.com/svg.latex?%5Csum_%7B%5Csubstack%7Bp%20%5Cle%20n%2C%20%5C%5C%5C%20p%20%5Ctext%7B%20prime%7D%7D%7D%20%5Cfrac%20n%20p%20%3D%20n%20%5Ccdot%20%5Csum_%7B%5Csubstack%7Bp%20%5Cle%20n%2C%20%5C%5C%5C%20p%20%5Ctext%7B%20prime%7D%7D%7D%20%5Cfrac%201%20p)

Let's recall two known facts.

  - The number of prime numbers less than or equal to ![](https://latex.codecogs.com/svg.latex?n) is approximately ![](https://latex.codecogs.com/svg.latex?%5Ctiny%20%5Cfrac%20n%20%7B%5Cln%20n%7D) .
  - The ![](https://latex.codecogs.com/svg.latex?k)-th prime number approximately equals ![](https://latex.codecogs.com/svg.latex?k%20%5Cln%20k) (that follows immediately from the previous fact).

Thus we can write down the sum in the following way:

![](https://latex.codecogs.com/svg.latex?%5Csum_%7B%5Csubstack%7Bp%20%5Cle%20n%2C%20%5C%5C%5C%20p%20%5Ctext%7B%20prime%7D%7D%7D%20%5Cfrac%201%20p%20%5Capprox%20%5Cfrac%201%202%20&plus;%20%5Csum_%7Bk%20%3D%202%7D%5E%7B%5Cfrac%20n%20%7B%5Cln%20n%7D%7D%20%5Cfrac%201%20%7Bk%20%5Cln%20k%7D)

Here we extracted the first prime number 2 from the sum, because ![](https://latex.codecogs.com/svg.latex?k%20%3D%201) in approximation ![](https://latex.codecogs.com/svg.latex?k%20%5Cln%20k) is ![](https://latex.codecogs.com/svg.latex?0) and causes a division by zero.

Now, let's evaluate this sum using the integral of a same function over ![](https://latex.codecogs.com/svg.latex?k) from ![](https://latex.codecogs.com/svg.latex?2) to ![](https://latex.codecogs.com/svg.latex?%5Ctiny%20%5Cfrac%20n%20%7B%5Cln%20n%7D) (we can make such approximation because, in fact, the sum is related to the integral as its approximation using the rectangle method):

![](https://latex.codecogs.com/svg.latex?%5Csum_%7Bk%20%3D%202%7D%5E%7B%5Cfrac%20n%20%7B%5Cln%20n%7D%7D%20%5Cfrac%201%20%7Bk%20%5Cln%20k%7D%20%5Capprox%20%5Cint_2%5E%7B%5Cfrac%20n%20%7B%5Cln%20n%7D%7D%20%5Cfrac%201%20%7Bk%20%5Cln%20k%7D%20dk)

The antiderivative for the integrand is ![](https://latex.codecogs.com/svg.latex?%5Cln%20%5Cln%20k). Using a substitution and removing terms of lower order, we'll get the result:

![](https://latex.codecogs.com/svg.latex?%5Cint_2%5E%7B%5Cfrac%20n%20%7B%5Cln%20n%7D%7D%20%5Cfrac%201%20%7Bk%20%5Cln%20k%7D%20dk%20%3D%20%5Cln%20%5Cln%20%5Cfrac%20n%20%7B%5Cln%20n%7D%20-%20%5Cln%20%5Cln%202%20%3D%20%5Cln%28%5Cln%20n%20-%20%5Cln%20%5Cln%20n%29%20-%20%5Cln%20%5Cln%202%20%5Capprox%20%5Cln%20%5Cln%20n)

Now, returning to the original sum, we'll get its approximate evaluation:

![](https://latex.codecogs.com/svg.latex?%5Csum_%7B%5Csubstack%7Bp%20%5Cle%20n%2C%20%5C%5C%5C%20p%5C%20is%5C%20prime%7D%7D%20%5Cfrac%20n%20p%20%5Capprox%20n%20%5Cln%20%5Cln%20n%20&plus;%20o%28n%29)

You can find a more strict proof (that gives more precise evaluation which is accurate within constant multipliers) in the book authored by Hardy & Wright "An Introduction to the Theory of Numbers" ![](https://latex.codecogs.com/svg.latex?%28p.%20349%29).

## Different optimizations of the Sieve of Eratosthenes

The biggest weakness of the algorithm is, that it "walks" along the memory multiple times, only manipulating single elements.
This is not very cache friendly.
And because of that, the constant which is concealed in ![](https://latex.codecogs.com/svg.latex?O%28n%20%5Clog%20%5Clog%20n%29) is comparably big.

Besides, the consumed memory is a bottleneck for big ![](https://latex.codecogs.com/svg.latex?n).

The methods presented below allow us to reduce the quantity of the performed operations, as well as to shorten the consumed memory noticeably.

### Sieving till root

Obviously, to find all the prime numbers until ![](https://latex.codecogs.com/svg.latex?n), it will be enough just to perform the sifting only by the prime numbers, which do not exceed the root of ![](https://latex.codecogs.com/svg.latex?n).

```cpp
int n;
vector<char> is_prime(n+1, true);
is_prime[0] = is_prime[1] = false;
for (int i = 2; i * i <= n; i++) {
    if (is_prime[i]) {
        for (int j = i * i; j <= n; j += i)
            is_prime[j] = false;
    }
}
```

Such optimization doesn't affect the complexity (indeed, by repeating the proof presented above we'll get the evaluation ![](https://latex.codecogs.com/svg.latex?n%20%5Cln%20%5Cln%20%5Csqrt%20n%20&plus;%20o%28n%29), which is asymptotically the same according to the properties of logarithms), though the number of operations will reduce noticeably.

### Sieving by the odd numbers only

Since all even numbers (except ![](https://latex.codecogs.com/svg.latex?2)) are composite, we can stop checking even numbers at all. Instead, we need to operate with odd numbers only.

First, it will allow us to half the needed memory. Second, it will reduce the number of operations performing by algorithm approximately in half.

### Reducing consumed memory

We should notice that algorithm of Eratosthenes operates with ![](https://latex.codecogs.com/svg.latex?n) bits of memory. Hence, we can essentially reduce consumed memory by preserving not ![](https://latex.codecogs.com/svg.latex?n) bytes, which are the variables of Boolean type, but ![](https://latex.codecogs.com/svg.latex?n) bits, i.e. ![](https://latex.codecogs.com/svg.latex?%5Ctiny%20%5Cfrac%20n%208) bytes of memory.

However, such an approach, which is called **bit-level compression**, will complicate the operations with these bits. Read or write operation on any bit will require several arithmetic operations and ultimately slow down the algorithm.

Thus, this approach is only justified, if ![](https://latex.codecogs.com/svg.latex?n) is so big that we cannot allocate ![](https://latex.codecogs.com/svg.latex?n) bytes of memory anymore.
In this case we will trade saving memory (![](https://latex.codecogs.com/svg.latex?8) times less) with significant slowing down of the algorithm.

After all, it's worth mentioning there exist data structures that automatically do a bit-level compression, such as `vector<bool>` and `bitset<>` in C++.

### Segmented Sieve

It follows from the optimization "sieving till root" that there is no need to keep the whole array `is_prime[1...n]` at all time.
For performing of sieving it's enough to keep just prime numbers until root of ![](https://latex.codecogs.com/svg.latex?n), i.e. `prime[1... sqrt(n)]`, split the complete range into blocks, and sieve each block separately.
In doing so, we never have to keep multiple blocks in memory at the same time, and the CPU handles caching a lot better.

Let ![](https://latex.codecogs.com/svg.latex?s) be a constant which determines the size of the block, then we have ![](https://latex.codecogs.com/svg.latex?%5Ctiny%20%5Clceil%20%7B%5Cfrac%20n%20s%7D%20%5Crceil) blocks altogether, and the block ![](https://latex.codecogs.com/svg.latex?k) (![](https://latex.codecogs.com/svg.latex?%5Ctiny%20k%20%3D%200%20...%20%5Clfloor%20%7B%5Cfrac%20n%20s%7D%20%5Crfloor)) contains the numbers in a segment ![](https://latex.codecogs.com/svg.latex?%5Bks%3B%20ks%20&plus;%20s%20-%201%5D).
We can work on blocks by turns, i.e. for every block ![](https://latex.codecogs.com/svg.latex?k) we will go through all the prime numbers (from ![](https://latex.codecogs.com/svg.latex?1) to ![](https://latex.codecogs.com/svg.latex?%5Csqrt%20n)) and perform sieving using them.
It is worth noting, that we have to modify the strategy a little bit when handling the first numbers: first, all the prime numbers from ![](https://latex.codecogs.com/svg.latex?%5B1%3B%20%5Csqrt%20n%5D)  shouldn't remove themselves; and second, the numbers ![](https://latex.codecogs.com/svg.latex?0) and ![](https://latex.codecogs.com/svg.latex?1) should be marked as non-prime numbers.
While working on the last block it should not be forgotten that the last needed number ![](https://latex.codecogs.com/svg.latex?n) is not necessary located in the end of the block.

Here we have an implementation that counts the number of primes smaller than or equal to ![](https://latex.codecogs.com/svg.latex?n) using block sieving.

```cpp
int count_primes(int n) {
    const int S = 10000;

    vector<int> primes;
    int nsqrt = sqrt(n);
    vector<char> is_prime(nsqrt + 1, true);
    for (int i = 2; i <= nsqrt; i++) {
        if (is_prime[i]) {
            primes.push_back(i);
            for (int j = i * i; j <= nsqrt; j += i)
                is_prime[j] = false;
        }
    }

    int result = 0;
    vector<char> block(S);
    for (int k = 0; k * S <= n; k++) {
        fill(block.begin(), block.end(), true);
        int start = k * S;
        for (int p : primes) {
            int start_idx = (start + p - 1) / p;
            int j = max(start_idx, p) * p - start;
            for (; j < S; j += p)
                block[j] = false;
        }
        if (k == 0)
            block[0] = block[1] = false;
        for (int i = 0; i < S && start + i <= n; i++) {
            if (block[i])
                result++;
        }
    }
    return result;
}
```

The running time of block sieving is the same as for regular sieve of Eratosthenes (unless the size of the blocks is very small), but the needed memory will shorten to ![](https://latex.codecogs.com/svg.latex?O%28%5Csqrt%7Bn%7D%20&plus;%20S%29) and we have better caching results.
On the other hand, there will be a division for each pair of a block and prime number from ![](https://latex.codecogs.com/svg.latex?%5B1%3B%20%5Csqrt%7Bn%7D%5D), and that will be far worse for smaller block sizes.
Hence, it is necessary to keep balance when selecting the constant ![](https://latex.codecogs.com/svg.latex?S).
We achieved the best results for block sizes between ![](https://latex.codecogs.com/svg.latex?10%5E4) and ![](https://latex.codecogs.com/svg.latex?10%5E5).

## Find primes in range

Sometimes we need to find all prime numbers in a range ![](https://latex.codecogs.com/svg.latex?%5BL%2CR%5D) of small size (e.g. ![](https://latex.codecogs.com/svg.latex?R%20-%20L%20&plus;%201%20%5Capprox%201e7)), where ![](https://latex.codecogs.com/svg.latex?R) can be very large (e.g. ![](https://latex.codecogs.com/svg.latex?1e12)).

To solve such a problem, we can use the idea of the Segmented sieve.
We pre-generate all prime numbers up to ![](https://latex.codecogs.com/svg.latex?%5Csqrt%20R), and use those primes to mark all composite numbers in the segment ![](https://latex.codecogs.com/svg.latex?%5BL%2C%20R%5D).

```cpp
vector<bool> segmentedSieve(long long L, long long R) {
    // generate all primes up to sqrt(R)
    long long lim = sqrt(R);
    vector<bool> mark(lim + 1, false);
    vector<long long> primes;
    for (long long i = 2; i <= lim; ++i) {
        if (!mark[i]) {
            primes.emplace_back(i);
            for (long long j = i * i; j <= lim; j += i)
                mark[j] = true;
        }
    }

    vector<bool> isPrime(R - L + 1, true);
    for (long long i : primes)
        for (long long j = max(i * i, (L + i - 1) / i * i); j <= R; j += i)
            isPrime[j - L] = false;
    if (L == 1)
        isPrime[0] = false;
    return isPrime;
}
```
Time complexity of this approach is ![](https://latex.codecogs.com/svg.latex?O%28%28R%20-%20L%20&plus;%201%29%20%5Clog%20%5Clog%20%28R%29%20&plus;%20%5Csqrt%20R%20%5Clog%20%5Clog%20%5Csqrt%20R%29).

It's also possible that we don't pre-generate all prime numbers:

```cpp
vector<bool> segmentedSieveNoPreGen(long long L, long long R) {
    vector<bool> isPrime(R - L + 1, true);
    long long lim = sqrt(R);
    for (long long i = 2; i <= lim; ++i)
        for (long long j = max(i * i, (L + i - 1) / i * i); j <= R; j += i)
            isPrime[j - L] = false;
    if (L == 1)
        isPrime[0] = false;
    return isPrime;
}
```

Obviously, the complexity is worse, which is ![](https://latex.codecogs.com/svg.latex?O%28%28R%20-%20L%20&plus;%201%29%20%5Clog%20%28R%29%20&plus;%20%5Csqrt%20R%29). However, it still runs very fast in practice.

## Linear time modification

We can modify the algorithm in a such a way, that it only has linear time complexity.
This approach is described in the article [Sieve of Eratosthenes Having Linear Time Complexity](./algebra/prime-sieve-linear.html).
However, this algorithm also has its own weaknesses.
