Excited to share that my first open-source pull request was merged into `FallByte`! 🎉 My change helps the app keep compressing or converting media files even when one file fails.

**The Issue:** An unexpected error in one file could stop the entire batch. The remaining files were left unprocessed, even though the app could still show a success message.

**How I Solved It:** I used Python and pytest to recreate the bug with three files, deliberately making the middle one fail. Then I changed how FallByte handles errors so it records the failed file and keeps going. The same test passed after the fix: two files succeeded, and one was correctly counted as failed. I learned how a small, focused test can prove that a fix works.

Big thanks to `@brunothinker` for reviewing my contribution and suggesting a way to simplify the error handling across the app.

**Check out the merged PR:** https://github.com/brunothinker/FallByte/pull/73

#OpenSource #GitHub #Python #SoftwareTesting #SoftwareEngineering #Coding
