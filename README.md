# better-dynlib

better-dynlib is a drop-in `std.DynLib` replacement that adds Windows support for loading dynamic libraries.

## add to project
requires zig `0.16.0`

to use this with the zig build system, import as so:
```bash
zig fetch --save git+https://github.com/eggyengine/better-dynlib
```

alternatively, you can just copy [root.zig](src/root.zig) directly into your codebase. 

and then in `build.zig`:
```zig
const dynlib = b.dependency("better_dynlib", .{
    .target = target,
});

exe.root_module.addImport("better_dynlib", dynlib.module("better_dynlib"));
```

and lastly in your library/executable:
```zig
const DynLib = @import("better_dynlib").DynLib;

var lib = try DynLib.open("mylib.dll");
defer lib.close();

const add = lib.lookup(*const fn (i32, i32) callconv(.c) i32, "add") orelse return error.SymbolNotFound;
```
