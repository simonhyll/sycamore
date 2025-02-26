# Control Flow

Control flow in Sycamore can be achieved using the interpolation syntax. For example:

```rust
let visible = create_signal(cx, true);

view! { cx,
    div {
        (if *visible.get() {
            view! { cx, "Now you see me" }
        } else {
            view! { cx, } // Now you don't
        })
    }
}
```

Since the interpolated value subscribes to `visible`, the content inside the if else will be toggled
when `visible` is changed.

The conditions for displaying content can also be more complex. For example, the following snippet
will display the value of `name` when it is non-empty, other wise displaying `"World"`.

Note the usage of `create_selector` here. The reason for this is to memoize the value of
`name.get().is_empty()`. We don't want the inner `view!` (inside the `if` block) to be recreated
every time `name` is changed. Rather, we only want it to be created when `name` becomes non-empty.

```rust
let name = create_signal(cx, String::new());
let is_empty = create_selector(cx, || !name.get().is_empty());

view! { cx,
    h1 {
        (if *is_empty.get() {
            view! { cx, span { (name.get()) } }
        } else {
            view! { cx, span { "World" } }
        })
    }
}
```
