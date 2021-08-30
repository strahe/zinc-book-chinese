# 结构体

结构体是一种自定义数据类型，可让你命名并将构成有意义的多个相关值打包在一起。 
结构体允许你轻松构建复杂的数据类型，并以尽可能少的冗长将它们传递到您的代码中。

可以通过点运算符访问结构体字段，详细解释在[这里](../../04-operators/06-access.md).

```rust,no_run,noplaypen
struct Person {
    age: u8,
    id: u64,
}

fn main() {
    let mut person = Person {
        age: 24,
        id: 123456789 as u64,
    };
    person.age = 25;
}
```

## 实现

可以实现一个结构体，即可以为其声明一些方法和相关项。结构实现类似于面向对象语言中类的行为部分。

```rust,no_run,noplaypen
struct Arithmetic {
    a: field,
    b: field,
}

impl Arithmetic {
    pub fn add(self) -> field {
        self.a + self.b
    }

    pub fn sub(self) -> field {
        self.a - self.b
    }

    pub fn mul(self) -> field {
        self.a * self.b
    }

    pub fn div(self) {
        require(false, "Field division is forbidden!");
    }
}

fn main() {
    let a: field = 10;
    let b: field = 5;
    let arithmetic = Arithmetic { a: a, b: b };
    
    dbg!("{} + {} = {}", a, b, arithmetic.add());
    dbg!("{} - {} = {}", a, b, arithmetic.sub());
    dbg!("{} * {} = {}", a, b, arithmetic.mul());
    dbg!("{} / {} = {}", a, b, arithmetic.div()); // will panic
}
```

有关方法的更多信息，请参阅这个[章节](../03-functions.md).
