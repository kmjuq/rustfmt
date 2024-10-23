# Rustfmt 使用
在 `cargo` 管理过程中，可以通过 `rustfmt` 来对代码进行格式化，但是默认的格式可能不符合管理要求，而官方文档感觉描述不是很通顺，因此想通过该项目记录一下各个参数实际作用。

在项目根目录创建 `rustfmt.toml` 文件即可对影响项目代码格式化。

## 参数项
目前`stable`的参数项不多，本人可能会调整的参数如下：

```toml
# 每行代码最大宽度，默认为100
max_width = 100
# 数组字面量宽度，会受 max_width 影响，50 意为 max_width * 50%
array_width = 50
# 注释最大宽度
comment_width = 100
# 方法参数布局调整 "Compressed", "Tall", "Vertical"，默认为 "Tall"
fn_params_layout = "Vertical"
# derive 多个派生宏是否合并，false为不合并，默认为 true
merge_derives = false
# 换行符  "Auto", "Native", "Unix", "Windows"
newline_style = "Unix"
# 方法调用时，采用 Vertical 样式时，所需的宽度
fn_call_width = 80
```

### max_width
```rust
fn max_width_more_than_100(
    counter1: Arc<Mutex<Vec<(i32, u64, String)>>>,
    counter2: Arc<Mutex<Vec<(i32, u64, String)>>>,
) {
}

fn max_width_less_than_100(counter1: Arc<Mutex<Vec<i32>>>, counter2: Arc<Mutex<Vec<i32>>>) {}
```

### array_width
```rust
// 字面量宽度少于 50 时
let array_literal_less_than_50 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 8, 7, 6, 5, 4, 3, 2, 1];
// 字面量宽度大于 50 时
let array_literal_more_than_50 = [
    1, 2, 3, 4, 5, 6, 7, 8, 9, 8, 7, 6, 5, 4, 3, 2, 1, 2,
];
// 字面量宽度大于 50 时，且超过 100 时会分行
let array_literal_more_than_50_max_width_100 = [
    1, 2, 3, 4, 5, 6, 7, 8, 9, 8, 1, 1, 2, 2, 3, 4, 5, 6, 6, 7, 9, 1, 2, 4, 5, 5, 4, 4, 5, 3,
    3, 5, 8, 5, 3, 6, 8, 2,
];
```

### fn_params_layout
#### fn_params_layout_Tall
此选项为默认选项
```rust
// 函数定义时，未超过 max_width，则参数定义在一行中
fn fn_params_layout_Tall_less_than_max_width(i81: i8, i161: i16, i321: i32, i641: i64, u81: u8) {}

// 函数定义时，超过 max_width，则参数定义在多行中
fn fn_params_layout_Tall_more_than_max_width(
    i81: i8,
    i161: i16,
    i321: i32,
    i641: i64,
    u81: u8,
    u161: u16,
) {
}
```
#### fn_params_layout_Compressed
```rust
// 函数定义时，未超过 max_width，则参数定义在一行中
fn fn_params_layout_Compressed_less_than_max_width(i81: i8, i161: i16, i321: i32, i641: i64) {}

// 函数定义时，超过 max_width，则参数定义在多行中
fn fn_params_layout_Compressed_more_than_max_width(
    i81: i8, i161: i16, i321: i32, i641: i64, u81: u8, u161: u16,
) {
}

// 函数定义时，超过 max_width，则多余的参数在多行中
fn fn_params_layout_Compressed_more_than_max_width_twice(
    i1: i64, i2: i64, i3: i64, i4: i64, i5: i64, i6: i64, i7: i64, i8: i64, i9: i64, i10: i64,
    i11: i64, i12: i64,
) {
}
```

#### fn_params_layout_Vertical
```rust
// 函数定义时，每个参数一行
fn fn_params_layout_Vertical_less_than_max_width(
    i81: i8,
    i161: i16,
    i321: i32,
    i641: i64,
) {
}

// 函数定义时，每个参数一行
fn fn_params_layout_Vertical_more_than_max_width(
    i81: i8,
    i161: i16,
    i321: i32,
    i641: i64,
    u81: u8,
    u161: u16,
) {
}
```
### merge_derives
```rust
#[derive(Eq, PartialEq)]
#[derive(Debug)]
#[derive(Copy, Clone)]
pub enum Foo {}
//默认为true，当是如上代码时，会自动格式化为如下代码，讲 derives 合并
#[derive(Eq, PartialEq, Debug, Copy, Clone)]
pub enum Foo {}
```

### newline_style
不知道怎么生效

## 附录
[官方参数查询](https://rust-lang.github.io/rustfmt)