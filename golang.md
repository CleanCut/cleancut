# Go stuff to remember

I use Go at my day job. Here's some random stuff I want to remember.

- Never use bare (aka naked) `return` keywords with a function that returns values. Bare `return`s force you to run the whole function in your head to figure out what's being returned, and humans are terrible at that.
- Put the type on every function parameter, it's clearer: `func(id int, iterations int)`, not `func(id, iterations int)`
- Don't use `goto` except to break out of (or continue) a nested loop, and that should be exceedingly rare.
- Always use the company `GOPROXY` if you're doing work stuff.
- Avoid single-letter variable names as much as you can, unless it's a tiny loop. Three- or five- or even eight letter variable names ain't gonna kill anyone, sheesh.
- Since Go doesn't have enums, use package level constants instead. You can newtype them if you're afraid folks will mix them with other values of the type, but it comes with the cost of having to cast the newtype to the base type to compare them.
- Go tooling has backward defaults. Feed any command `./...` to have it operate on everything like it ought to have in the first place.
- The folks who create Go don't care about ergonomics in tooling -- don't bother asking for or suggesting improvements, they will be interpreted as a sign that you are not a Real Gopher™ and dismissed as unnecessary.  Bash, Makefiles, etc. are the workaround.
- Test functions start with `Test` and take a `*testing.T`
- Benchmark functions start with `Benchmark` and take a `*testing.B`, iterate from `0` to `b.N`
- The one place you can use an underscore is in the name of a test testing an unexported function `Test_someFunction`
- Use `t.Fatal[f]` to print/return an error in a test. `t.Error[f]` will not return from the function.
- Use a `TestMain` function if you need to set up / tear down stuff for the tests (only one per package).
- `*testing.T.Cleanup()` is like `defer`, but you can pass the `*testing.T` to other functions and call `Cleanup()` there and the cleanup happens at the end of the test.
- 
- During a test, the current directory is the package directory of the current package.
- Always put test data in a directory `testdata/` relative to the package directory of the test that uses it.
- Avoid creating package-level variables.
- Unit tests go in the same file as what they're testing.
- Make a `packagename_test.go` file in the same directory for integration tests, but it will be `package packagename_test` and you'll have to `import full/path/to/package`
- Use `cmp.Diff` and `cmp.Comparer` from the `github.com/google/go-cmp/cmp` package to compare structs, maps, and slices in tests.
- Use `errors.Is` and `errors.As` to check error types
- Use `reflect.DeepEqual` for comparing maps and slices
- Interface values are wide pointers. If both pointers are `nil`, then the interface value is `nil`, but if the interface value is a `nil` of some type (the type pointer is not `nil`), then the interface _value_ is not `nil`. This is why code that uses interface values should be written so it works with `nil` values...because it's hard to tell if the interface's value (via the value pointer) is `nil` or not. (The only way is through reflection).
