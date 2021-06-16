#  GTest CMake Example Project
This is a scaffold for C/C++ development.
Its features include: 

- Unit Test with GTest
- (optional) Mocking with GMock
- (optional) Benchmark with Google's benchmark
- Prebuilt CMake config

# Prerequisites 

- `gtest` installed
- (optional) `gmock` installed
- (optional) `benchmark` installed
- `pthread` installed
## Installing dependencies on Ubuntu

```shell
$ apt install libgtest-dev libpthread-stubs0-dev
$ apt install libbenchmark-dev libgmock-dev # optional
```

# Adding new tests

1. Create a new file at `test` folder (use the .cpp or .c extension);
1. Include UUT (unit under test) header file from the `src` folder
1. Include gtest using `#include <gtest/gtest.h>`
1. Create your first test case:

```c++
TEST(TestClass, TestCaseName) {
    int value[] = {1,8,14,22};
    int weight[] = {4,12,20,30};
    auto obj = SourceClass(4, value, weight);
    int obtainedResult = obj.compute();
    int expectedResult = 36;
    ASSERT_EQ(obtainedResult, expectedResult);
}
```
OR using SetUp and TearDown:
```c++
class TestClass : public ::testing::Test {
protected:
    virtual void SetUp () {
        value = {1,8,14,22};
        weight = {4,12,20,30};
        obj = SourceClass(4, value, weight);
    }
    
    virtual void TearDown() {
        // Do nothing...
    }

    int value[4];
    int weight[4];
    SourceClass obj;
};

// Test with fixture
TEST_F(TestClass, TestCaseName) {
    int obtainedResult = obj.compute();
    int expectedResult = 36;
    ASSERT_EQ(obtainedResult, expectedResult);
}
```

# Adding benchmarks

1. Create a new file at `test` folder (use the .cpp or .c extension);
1. Include UUT (unit under test) header file from the `src` folder;
1. Include gtest using `#include <gtest/gtest.h>`;
1. Include benchmark using `#include "benchmark/benchmark.h"`;
1. Create your first benchmark.

```c++
static void BM_TestCase() {
    int value[] = {1,8,14,22};
    int weight[] = {4,12,20,30};
    SourceClass obj = SourceClass(4, value, weight);
    int obtainedResult = obj.compute();
    int expectedResult = 36;
    ASSERT_EQ(obtainedResult, expectedResult);
}

BENCHMARK(BM_TestCase);

TEST(TestClass, Benchmarks)
{
    ::benchmark::RunSpecifiedBenchmarks();
}
```
OR using SetUp and TearDown:
```c++
class TestClass : public benchmark::Fixture {
public:
    
    TestClass(): obj() {}
    
protected:
    virtual void SetUp () {
        int a[4] = {1,8,14,22};
        int b[4] = {4,12,20,30};
        obj = SourceClass(4, a, b);
    }
    
    virtual void TearDown() {
        // Do nothing...
    }
    
    SourceClass obj;
};

BENCHMARK_F(TestClass, BM_TestCase)(benchmark::State& state){
    for (auto _ : state) {
        obj.compute();
    }
}
```