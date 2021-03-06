# Mersenne Twister Predictor

[![Travis](https://img.shields.io/travis/kmyk/mersenne-twister-predictor.svg)](https://travis-ci.org/kmyk/mersenne-twister-predictor)
![PyPI](https://img.shields.io/pypi/l/mersenne-twister-predictor.svg)
![PyPI](https://img.shields.io/pypi/pyversions/mersenne-twister-predictor.svg)
![PyPI](https://img.shields.io/pypi/status/mersenne-twister-predictor.svg)
[![PyPI](https://img.shields.io/pypi/v/mersenne-twister-predictor.svg)](https://pypi.python.org/pypi/mersenne-twister-predictor)

Predict MT19937 PRNG, from preceding 624 generated numbers.

## usage

### install

``` sh
$ pip3 install mersenne-twister-predictor
```

### as a library

For the documentation, see `mt19937predictor.py` and `tests/random.py`.

### as a command

Give the 624 32-bit integers, as unsigned decimal integers, line by line.

``` sh
$ wc data.txt
 624  624 6696 data.txt

$ head -n 4 data.txt
734947730
363401994
806921074
790218357

$ cat data.txt | mt19937predict > predicted.txt
```

## example (same as `tests/cplusplus.py`)

This is an example of a pseudorandom number generator using the Mersenne Twister.

``` c++
// generator.cpp
#include <iostream>
#include <random>
using namespace std;
int main() {
    random_device seed_gen;
    mt19937 mt(seed_gen());
    for (int i = 0; i < 1000; ++i) {
        cout << mt() << endl;
    }
    return 0;
}
```

Compile it and let it generate 1000 numbers.

``` sh
$ g++ -std=c++11 generator.cpp

$ ./a.out > data.txt

$ head data.txt
734947730
363401994
806921074
790218357
1766244801
680628322
1972477509
4015123394
2848130362
3481789813
```

Take the 624 consecutive numbers.  We will predict the rest, 376 numbers.

``` sh
$ head -n 624 data.txt > known.txt

$ tail -n 376 data.txt > correct.txt

$ wc known.txt
 624  624 6696 known.txt
```

Predict.

``` sh
$ cat known.txt | mt19937predict | head -n 376 > predicted.txt
```

Compare the predict numbers and the original ones.
If `diff` outputs nothing, it means the prediction was succeeded.

``` sh
$ diff predicted.txt correct.txt
```

## reference

thanks to

-   <http://www.math.sci.hiroshima-u.ac.jp/~m-mat/MT/mt.html>
-   <http://homepage1.nifty.com/herumi/diary/1505.html#18>
-   <http://d.hatena.ne.jp/oupo/20141016/>
