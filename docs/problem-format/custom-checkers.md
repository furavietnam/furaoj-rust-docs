# Custom Output Checkers

For problems allowing multiple valid answers, floating point tolerance, or arbitrary graph representations, problem authors can provide a custom checker.

## Testlib Interface (C++)

Custom checkers are compiled against standard `testlib.h` conventions:

```cpp
#include "testlib.h"

int main(int argc, char* argv[]) {
    setName("Compare floating point values with 1e-6 precision");
    registerTestlibCmd(argc, argv);

    double expected = ans.readDouble();
    double contestant = ouf.readDouble();

    if (fabs(expected - contestant) > 1e-6) {
        quitf(_wa, "Expected %.6f, found %.6f", expected, contestant);
    }

    quitf(_ok, "Answer matches within tolerance");
}
```

## Checker Exit Codes

- `0` (`_ok`): Accepted (`AC`)
- `1` (`_wa`): Wrong Answer (`WA`)
- `2` (`_pe`): Presentation Error
- `3` (`_fail`): Internal Error (`IE`)
