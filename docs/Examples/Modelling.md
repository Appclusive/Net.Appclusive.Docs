# Note

For a more technical description of *Models* see [BlueprintEngine](../BlueprintEngine.md).

# Introduction

A `Model` is a formal description of an object which includes

* `ModelAttributes`
* `ModelStates`
* `ModelActions`

A `Model` can (and must) *derive* from other `Models`, but only from one `Model` at a time. The `Model` from where we *derive* is called the `Parent`. Each *Parent* in turn can have another *Parent*. By *deriving* from a `Parent` a `Model` tells use that it shares some similarity with its *Parent*. 

Any customer (or `Tenant` which is the technical term in Appclusive to represent a separate legal entity in the inventory) has a *name* and *domain name* which serves as the base identifying information for all objects.

Let's try an example (where we work as the `Tenant` *example.com*):

Presume we have a *Shape*. And a *Square* and a *Circle* which are *Shapes* as well. Now these two shapes do not have that much in common, except that both have an *Area* (which would then become a common `ModelAttribute` of all *Shapes*). And we want this *Area* to be the combined surface in *acres* of any respective shape (we have *very* large shapes).

So we can start defining a `Model` *Shap* like this:

``` csharp
public class Shape : BaseModel
{
}
```

In this case our *Shape* is a completely new `Model` that does not want to have any similarities with other `Models`. But as any `Model` **must** derive from a *Parent* it derives from `BaseModel` which serves as a common base for all models so they can be identifies as such. *Deriving* from a `Model` is always expressed by writing the `Model` after our new `Model` delimited by a *colon* (`:`).

In order to define our common `Attribute` *Area* we extend the *Shape* like this:

``` csharp
public class Shape : BaseModel
{
  [Unit("acre")]
  [AttributeName("com.example.Shape.Area")]
  public float Area { get; set; }
}
```

Here we see a few intersting things:

1. public float Area { get; set; }

  By describing our *Area* as **public** we tell Appclusive that the *Area* `ModelAttribute` should be visible everyone to everyone. In addition, we make the *Area* a **float** so it can contain floating point numbers (like 1.32). If we only wanted odd or even number we could have defined *Area* as **int**. This **float** is called the *Type* of the `ModelAttribute`. Furthermore to have the *Area* visible to everyone we tell Appclusive that *Area* is potentially readable *and* writable (as long as we have enough permissions to do that - remember: Appclusive has a fine grained permission model).

  Most of the time we use **public** and **{ get; set; }** in our `ModelAttributes`, we nevertheless have to write it down.
  
2. [AttributeName("com.example.Shape.Area")]

  As *Area* is a relatively common name we want to uniquely describe our `ModelAttribute` so others can identify this `ModelAttribute` was defined by us. We therfore mark *Area* with a **full qualified name** (or **FQCN** for short) that is constructed from our `Tenant` name *example.com*. At Appclusive these names are always created in reverse order (so *com.example.Shape.Area* instead of *example.com.Shape.Area* so the can be nicely ordered by each `Tenant` name). By using the `Tenant` name as a name *prefix* (which is unique throughout the system) we guarantee uniqueness of the `ModelAttributes` as well.
  
3. [Unit("acre")]

  As noted before we want the *Are* to be calculated and count in *acres* so we give a hint to any user and possible designer be specifying a *unit* on our `ModelAttribute`. Of course this is optional and we do not have to define a *unit*.

What we learned so far:

Any `ModelAttribute` we define on a `Model` consists of at least two elements/lines: an `AttributeName` and type definition and its visibility.

Now back to our example. We want to additional shapes: *Circle* and *Square*, so let's define them:


``` csharp
public class Circle : Shape
{
}

public class Square : Shape
{
}
```

Again this looks familiar to the definition of the *Shape*. But there is a difference: as want to *derive* from *Shape* we use `Shape` instead of `BaseModel` after the colon. And by deriving from *Shape* we tell Appclusive that we want to use the `ModelAttribute` *Area* as well on our two new models.

Now as our two models are different we define additional attributes on them:

``` csharp
public class Circle : Shape
{
  [AttributeName("com.example.Circle.Radius")]
  public float Radius { get; set; }
}

public class Square : Shape
{
  [AttributeName("com.example.Square.Length")]
  public float Length { get; set; }
}
```

There is nothing new in that for us. We use the mechanism as before.

Now we realise we want to be able to *resize* our shapes, and we therefore want to add a common `ModelAction` to all shapes. We extend *Shape* once more:

``` csharp
public class Shape : BaseModel
{
  [Unit("acre")]
  [AttributeName("com.example.Shape.Area")]
  public float Area { get; set; }
  
  public class Resize : ModelAction
  {
  }
}
```

As we can see, all we need to do is to define our *resize* `Action` (followed by `: ModelAction`) and Appclusive knows we want an `Action` in that model. Even better, now every model that *derives* from *Shape* also has that same `Action`. 

But hold it, something is missing: if we want to resize something, we have to know *how much* we want to resize it. So let's add another `ModelAttribute` but this time *inside* the `ModelAction`:

``` csharp
public class Shape : BaseModel
{
  [Unit("acre")]
  [AttributeName("com.example.Shape.Area")]
  public float Area { get; set; }
  
  public class Resize : ModelAction
  {
    [Required]
    [AttributeName("com.example.Shape.Resize.NewSize")]
    public float NewSize { get; set; }
  }
}
```

Note: in order to force our users to supply a *NewSize* when calling the *Resize* `Action` we mark the `ModelAttribute` as being `Required`. Appclusive will not process any *Resize* requests unless the user specifies a *NewSize*.

This is all good and fine but what does *NewSize* mean for a *Circle* or a *Shape*? The answer to this is: *it depends*. For the *Circle* we decide to use *NewSize* as the new *radius* and for the *Square* we decide to use *NewSize* as the new *Length*. 

This is something we call polymorphism. Both, *Circle* and *Square*, are a *Shape*, which means they have common ground. But in detail they might be more specialised and differ in their use and operation. But as long as we know how to resize a *Shape* (i.e. knowing the name of the `Action` and its `ModelAttributes`) we will also know how resize any other *derived* shape.

# Advanced Modelling

After a while happily playing around and resizing our shapes we realise we would also like to have a *Rhomboid* instead of only a *Square*. So what do we do?

We define a new `Model` that also derives from *Shape*:

``` csharp
public class Rhomboid: Shape
{
  [AttributeName("com.example.Rhomboid.Length1")]
  public float Length1 { get; set; }

  [AttributeName("com.example.Rhomboid.Length2")]
  public float Length1 { get; set; }
}
```

We gave the *Rhomboid* two `ModelAttributes` *Length1* and *Length2* so our rectangle can have two differing lengths. But what about our *Resize* `Action`? The action only has a single *NewSize* `Attribute`. How do we resize both attributes of our rhomboid? The answer is simple: We override the existing `Action`:

``` csharp
public class Rhomboid: Shape
{
  [AttributeName("com.example.Rhomboid.Length1")]
  public float Length1 { get; set; }

  [AttributeName("com.example.Rhomboid.Length2")]
  public float Length2 { get; set; }

  public new class Resize : Shape.Resize
  {
    [AttributeName("com.example.Rhomboid.Resize.Length2")]
    public float Length2 { get; set; }
  }
}
```

Here we see that we derived the existing `ModelAttribute` *NewSize* from the parent *Shape* as the Length1 parameter and added a new `ModelAttribute` *Length2*. The former attribute was marked as `Required` from its parent and the *Width* is effectively optional (as it is not marked as `Required`). Our *Rhomboid* will then use the value *NewSize* for *Length1* and *Length2* if no "Length2* was specified by the user. The benefit of this is that any user who know how to *Resize* a *Shape* still knows how to resize a *Rhomboid*.

And know we see that we can further re-arrange our existing *Square* as it has something in common with *Rhomboid*:

``` csharp
public class Square : Rhomboid
{
  public new class Resize : Shape.Rhomboid
  {
  }
}
```

Here we see that the definition of our *Square* has been simplified. We do not need any special *Length* property any more and the *Resize* `Action` is directly taken from the *Rhomboid*. A user who knows a *Shape* and only supplies the *NewSize* attribute can resize a shape and a user who supplies the second length2 attribute from the *Rhomboid* can also resize a *Square* (of course we will inform the user if both of the lengths do not match as a *Square* must have all the same lengths).

By this we can see that we can simplify and re-use components if we design our `Models` carefully.

This example could be further extended by supporting *Rectangle* as well (which would lead to yet another parent in our model chain).

## Behaviours

*WIP*
