# Rust Raytracer

Here I'll be documenting a personal guide for all new Rust concepts
learned via completing the [Raytracing in One Weekend](https://raytracing.github.io/books/RayTracingInOneWeekend.html)
written in C++ and creating a Rust Implmentation

## Basics of `struct`

When implementing a trait for a struct `Self` is just a type alias of the struct
`type Self = typeofstruct`

## Operator Oveload

For operations overloading you need to import the standard library `ops`
and then choose which operator you need to oveload for the `struct` and implement it via `impl`
e.g (Division, Multiplicaton ...)

Also some traits need you to use the type alias Output, we can output any type from the `impl` trait.

```
import std::ops;

impl ops::Add for Vec3 {
    type Output = // select which type you want to delete
    fn add(self, rhs: Self) -> Self::Output {
        ...
    }
}
// now we can do vec+vec
```

### Operator Oveload with Generics

By default an `ops` trait takes the same struct for its _rhs_ (**_right hand side_**) to make it posible to accept any other type of its _rhs_ we need to use generic of an `ops` trait (it can take a type or a ref).

```
impl ops::Add<f64> from Vec3{
    type Output = Self;

    fn add(self, rhs:f64){
        ...
    }
}
```

### Important Notes About Operator Overload trait

- Some trait in there implementation take the `self` that triggers a move. Some you can
<ul>
<ul>

**First** oveload the the reference of the struct.

```
impl ops::Add for &Vec3{
    fn add(&self, rhs:Vec3)
}

let sum_of_vec3 = &v + &v

```

</ul>
</ul>

<ul>
<ul>

**Second** Add the copy marcro, to the struct so implicitly any passing of the struct will be cloned (the copy macro needs the clone macro)

```
#[derice(Copy,Clone)]
struct Vec3;
fn main (){
    let v = Vec3::new();
    takes_vec3(v); // <- implicit takes_vec3(v.clone())
}

fn takes_vec3(v: Vec3);

```

</ul>
</ul>

- Operator Overload apply only at the left of the operation. Meaning if we oveloaded the **+** operator with f64 generic for Vec3, then we can only do vec3 + 1.0 and not 1.0 +vec3. This means we need to oveload f64 with the Vec3 generic.
<ul>
<ul>

```
impl ops::Add<Vec3> for f64 { //<-- f64 has copy already implemented
    type Output = Vec3

    fn add(self, rhs:Vec3){
        ...
    }
}
```

</ul>
</ul>

## Borrow and Ownership

_Future notes for any new things learned about borrow checker and ownership rules_

### Copy and Clone Trait
