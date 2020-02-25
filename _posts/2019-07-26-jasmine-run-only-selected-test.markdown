---
layout: post
title: 'Jasmine. Run selected tests only'
date: 2019-07-26
tags:
  - javascript
keywords:
  - jasmine run selected tests only
  - npm test is slow
---

I have faced the issue on the project. Couldn't do TDD, because running whole test suite is so slow. No chances to run single test, typescript needs to be compiled, npm install needs happend with each run, we need to build test coverage. I don't argue, but I need run my test fast in this Red-Green-Refactor cycle. Thanks Myk for help 🤗

<!--more-->

1. We need to have separate file for `jasmine.json` (I called it `jasmine_fast.js`)

```
{
    "spec_dir": "test",
    "spec_files": [
        "this/is/path/to/SelectedFile1.spec.ts",
        "this/is/path/to/SelectedFile2.spec.ts"
    ]
}
```

2. Simple script `run_tests.sh`

```
#!/bin/sh

time jasmine-ts --config=./jasmine_fast.json
```

don't forget to run `chmod +x run_test.sh`

## And here go results for

This script:

```JSON
{
    "test": "npm install && nyc --reporter lcov -e .ts --report-dir ./test/coverage jasmine-ts --config=jasmine.json && nyc report"
}
```

```bash
$ time npm test
```

```
219 specs, 5 failures
Finished in 0.525 seconds
Randomized with seed 07142 (jasmine --random=true --seed=07142)
npm ERR! Test failed.  See above for more details.

real	0m29.251s
user	0m35.840s <<<<
sys	0m2.828s
```

and `$./run_test.sh` for selected files only

```
15 specs, 5 failures
Finished in 0.102 seconds
Randomized with seed 92304 (jasmine --random=true --seed=92304)

real	0m5.256s
user	0m8.878s <<<<
sys	0m0.511s
```

Now I have an opportunity to run just single test file vs whole suite which saves me a lot of time!
