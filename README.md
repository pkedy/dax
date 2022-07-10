# ax

**Note:** This is very early stages. Just started working on it. A lot is not tested and won't work.

An automation toolkit for Deno inspired by [zx](https://github.com/google/zx).

Differences:

1. No globals or custom CLI command to execute—import into a script then use `deno run -A your_script.ts`.
1. Cross platform shell to help the code work on Windows.
   - Uses [deno_task_shell](https://github.com/denoland/deno_task_shell)'s parser.
   - This is very early stages, so I only have simple commands working at the moment and no cross platform commands.

## Example

```ts
// note: this is not published yet...
import $ from "https://deno.land/x/ax@{VERSION_GOES_HERE}/mod.ts";

const result = await $`deno eval 'console.log(5);'`;
console.log(result.stdout.trim()); // 5

// runs the script showing stdout (stderr is inherited by default)
await $`deno run my_script.ts`.stdout("inherit");

// capture stderr
const result = await $`deno eval 'console.error(5);'`.stderr("piped");
console.log(result.stderr.trim()); // 5, would throw if not piped

// sleep
await $.sleep(1000);

// change directory
$.cd("newDir");

// get path to an executable
await $.which("deno"); // path to deno executable

// attempt doing an action until it succeeds
await $.withRetries({
  count: 5,
  delay: 5_000,
  action: async () => {
    await $`cargo publish`;
  },
});

// re-export of deno_std's path
$.path.basename("./deno/std/path/mod.ts"); // mod.ts

// re-export of deno_std's fs
for await (const file of $.fs.expandGlob("**/*.ts")) {
  console.log(file);
}
```
