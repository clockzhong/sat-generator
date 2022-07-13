# SAT Generator

## Integer Factorization to SAT

The `PrimesProductToSAT.py` script generates a SAT instance from the known product $n=p\cdot q$ of two positive numbers $p$ and $q$ greater than 1. Generally speaking, $p$ and $q$ need not be primes.

If $p$ and $q$ are *distinct primes*, the generated SAT instance will have *exactly one solution* (we assume $p>q$).

Since our generator uses the Karatsuba multiplication algorithm recursively, the number of bits of each factor (and, hence, the product) must be a power of 2. Thus, we may extend the length of $p$, $q$, and $n$ to satisfy that condition. The benefit of the Karatsuba algorithm is that even for small $n$, we get asymptotically less SAT variables $\Theta(n^{\log_2{3}})=O(n^{1.58})$ compared to the traditional or Chinese multiplication with the  $\Theta(n^2)$ complexity.

## Usage

For known $p$ and $q$, invoke:

```bash
./PrimesProductToSAT.py <p> <q>
```

If you know only the product $n$, you need to specify also the expected number of bits for each of the two $n$ factors:

```bash
./PrimesProductToSAT.py <n> <number-of-bits-in-p> <number-of-bits-in-q>
```

The resulting file will be named `<n>.cnf` and will be stored according to the DIMACS format (a textual format for describing SAT instances in CNF). Look at the comments in that file: you will see which SAT variables will correspond to the bits of the two factors of $n$ after the SAT instance is solved.

> Warning! If you try to simplify the generated .cnf file (e.g., by `lingeling -s`), the names of the variables may change. In that case, it would be difficult to substitute variables with the bits of the factors of $n$.

## Testing

For testing purposes, you will need the `python-sat` library:

```bash
pip3 install python-sat
```

Then just play with `Test.py`.