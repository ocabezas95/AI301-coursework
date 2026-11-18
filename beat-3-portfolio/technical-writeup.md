# How I Kept FallByte Batches Running After a File Failed

## Overview

FallByte is an app that compresses media files (audio, video, images) to reduce their size and converts them between formats. The problem was that when one file raised an unexpected error, the batch stopped at that file, leaving the remaining files unprocessed, and the app could still show a success message even though it hadn't finished the batch. My contribution is that when the app now encounters an unexpected processing error, instead of stopping the whole batch, it marks that file as failed and continues processing the remaining files. The pull request is at https://github.com/brunothinker/FallByte/pull/73 and addresses issue #50. The PR has since been merged, and the issue is closed.

## What happened

I set up a test with three files: `a.png`, `b.png`, and `c.png`. I made `b.png` raise an error to see whether FallByte would still process `c.png`. I expected two successes and one failure, but pytest showed only one success. The batch stopped at `b.png` and never attempted `c.png`.

```text
E       assert 1 == 2
```

I moved error handling inside the loop so FallByte could mark the failed file and continue processing the rest of the batch. When I reran the same test, it passed: two files succeeded and one failed.

```text
1 passed in 0.17s
```

Before I opened the PR, the review caught a gap in my test: it checked the success and failure counts but didn’t verify that the error log identified the failed file. I added an assertion that the log included the path to `b.png`, then reran the test and confirmed it passed.

The maintainer merged `PR #73`, closing `issue #50`. For future work, they suggested moving the error handling repeated across all six controllers into a shared `BaseController` helper. The PR merged without that refactor.

## Limitations and what's next

**Testing across platforms:** All 241 batch and UI tests passed, but FallByte's FFmpeg setup prevented parts of the full suite from completing on my Mac. My next step is to run the full suite on Linux to verify the media processing paths I couldn't test locally.
