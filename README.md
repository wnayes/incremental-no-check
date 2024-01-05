Demo of an unusual behavior in the TypeScript compiler.

`large.ts` is supposed to not be checked due to `// @ts-nocheck`.
It doesn't get checked during an initial compile.
However, if some file it imports changes, and a subsequent incremental compile
is performed, the compiler spends CPU time type checking on the file. I don't think
diagnostics are reported from the checking, however the compiler is simply
performing extra work that seems unnecessary.

## Workflow

1. `npm install`
2. `npm run build` (does tsc invocation with `--generateTrace`)
3. Observe that `./traces/trace.json` does not contain any `checkSourceFile` entries for `large.ts`.
4. Change `other.ts` (change the number inside, etc.)
5. `npm run build`
6. Observe that the new `./traces/trace.json` does contain `checkSourceFile` for `large.ts`,
   as well as various other entries that indicate it did semantic analysis of the file.

---

I included `traces-1` and `traces-2` folders in the repository that show my trace logs from the above workflow. `traces-2` is from the second incremental compile that shouldn't have checked `large.ts`.
