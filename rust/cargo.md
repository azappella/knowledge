# cargo


### new project

```
cargo new
```

### build

This command creates an executable file in target/debug/hello_cargo

```
cargo build
```

### run

Using cargo run is more convenient than having to remember to run cargo build and then use the whole path to the binary, so most developers use cargo run.

```
cargo run
```

### check

This command quickly checks your code to make sure it compiles but doesn’t produce an executable.

```
cargo check
```


### release

When your project is finally ready for release, you can use cargo build --release to compile it with optimizations.
This command will create an executable in target/release instead of target/debug.

```
cargo build --release
```

### docs

This command will build documentation provided by all of your dependencies locally and open it in your browser

```cargo doc --open```
