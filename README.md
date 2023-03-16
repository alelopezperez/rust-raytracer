# Rust Raytracer

Here I'll be documenting a personal guide for all new Rust concepts
learned via completing the [Raytracing in One Weekend](https://raytracing.github.io/books/RayTracingInOneWeekend.html)
written in C++ and creating a Rust Implmentation

## Current Chapter

- 8.0 Diffuse

## Basics of `struct`

When implementing a trait for a struct `Self` is just a type alias of the struct
`type Self = typeofstruct`

when using impl if you dont use `self` or `&self` or `&mut self` the function will not for the instatiaded object, it will be an associated funciton `SomeStruct::func()`

## Statics and Const

**constants** are declared with the `const` keyword there are **inlined** in the program binary so it does not have an address in memory and need to be explictly anotated.

```
const infinity: f64 = f64::INFINITY;
const pi: f64 = 3.1415926535897932385;
```

**static** is a sort global varibles they are NOT inlined in the program so it does have an address in memory and it can be mutable (it is unsafe to mutate)

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

## `impl` and `traits`

Only impls of traits can be done on `&SomeStruct`

```
impl trait for &SomeStruct{
    fn function(){

    }
}
```

if you try impl `&SomeStruct` it will error, instead pass `&self` if you need to use the reference of the instatiated struct

```
impl SomeStruc {
    fn foo(&self){

    }

    fn bar(self){

    }

}
```

## Borrow and Ownership

_Future notes for any new things learned about borrow checker and ownership rules_

### Copy and Clone Trait

### Inlining

- https://nnethercote.github.io/perf-book/inlining.html

### Dyn and RC RefCell

When implementing something that the compilers does not know the size at compile, for this example a `Vec<TraitType>` we do not know the size the struct at compile time. At such we need to use **Dynamic Dispatch** via the `dyn` keyword.

- `Vec<impl TraitType>` impl trait generic is only for functions and return types.
- `Vec<Box<TraitType>>` this is ok, because the compiler knows the size of a pointer
- `Vec<Rc<TraitType>>` this is ok,
