# Introduction

Note: For a more technical description of *Models* see [BlueprintEngine](../BlueprintEngine.md).

A `Model` is a formal description of an object which includes

* `ModelAttributes`
* `ModelStates`
* `ModelActions`

A `Model` can (and must) *derive* from other `Models`, but only from one `Model` at a time. The `Model` from where we *derive* is called the *Parent*. Each *Parent* in turn has one other *Parent*. By *deriving* from a *Parent* a `Model` tells us, that it shares some similarity with it. 

Any customer (or `Tenant` which is the technical term in Appclusive to represent a separate legal entity in the inventory) has a *name* and *domain name* which serves as the base identifying information for all objects.

Let's try an example (where we work as the `Tenant` *example.com*):

Presume we want to model a *Circle* and a *Square*. Now these two objects do not have that much in common, except that both have an common `ModelAttribute` named *Area* (in the unit *acres*, as we have *very* large shapes). We would therefore define a *Shape* as the basis for our *Circle* and *Square*.

## Shape

So we can start defining a `Model` *Shape* like this:

``` csharp
public class Shape : BaseModel
{
}
```

In this case our *Shape* is a completely new `Model` that does not want to have any similarities with other `Models`. But as any `Model` **must** derive from a *Parent* it derives from `BaseModel` which serves as a common base for all models (so they can be identifies as such). *Deriving* from a `Model` is always expressed by specifying the *Parent* after our new `Model` delimited by a *colon* (`:`).

### ModelAttributes

In order to define our common `ModelAttribute` *Area* we extend the *Shape* like this:

``` csharp
public class Shape : BaseModel
{
  [Unit("acre")]
  [AttributeName("com.example.Shape.Area")]
  public float Area { get; set; }
}
```

Here we notice a few intersting things:

#### public float Area { get; set; }

 By describing our *Area* as **public** we tell Appclusive that the `ModelAttribute` *Area* should be visible everyone. In addition, we make the *Area* a **float** so it can contain floating point numbers (like 1.32). If we only wanted odd or even numbers we could have defined *Area* as **int**. This **float** is called the *Type* of the `ModelAttribute`. Furthermore, to have the *Area* visible to everyone we tell Appclusive that it is potentially **readable** *and* **writable** (as long as we have enough permissions to do that - remember: Appclusive has a fine grained permission model).

 Most of the time we use **public** and **{ get; set; }** in our `ModelAttributes`, we nevertheless have to write it down.
  
#### [AttributeName("com.example.Shape.Area")]

 As *Area* is a relatively common name we want to uniquely describe that `ModelAttribute`, so others can identify it was defined by us. We therefore mark *Area* with a **full qualified name** (or **FQCN** for short) that is constructed from our `Tenant` name *example.com*. At Appclusive these names are always created in reverse order (we use *com.example.Shape.Area* instead of *example.com.Shape.Area*, so it can be nicely ordered by each `Tenant` name). By using the `Tenant` name as a name *prefix* (which is unique throughout the system) we guarantee uniqueness of the `ModelAttributes` as well.
  
#### [Unit("acre")]

As noted before we want the *Are* to be calculated and count in *acres* so we give a hint to any user and possible designer be specifying a *unit* on our `ModelAttribute`. Of course this is optional and we do not have to define a *unit*.

What we learned so far:

Any `ModelAttribute` we define on a `Model` consists of at least two elements/lines: an `AttributeName` and type definition and its visibility.

## Circle and Square

Now back to our example. We actuall want our additional shapes *Circle* and *Square*, so let's define them:


``` csharp
public class Circle : Shape
{
}

public class Square : Shape
{
}
```

Again this looks familiar to the definition of *Shape*. But there is a difference: as we want to *derive* from *Shape* we use `Shape` instead of `BaseModel` after the colon. And by deriving from *Shape* we tell Appclusive that we want to use the `ModelAttribute` *Area* as well on our two new models.

### ModelAttributes

Now as our two models are different we define additional `ModelAttributes` on them:

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

There is nothing new in that for us. We used the same mechanism as with our basic *Shape*.

### ModelActions

Now we realise that we want to be able to *resize* our shapes, and we therefore want to add a common `ModelAction` to all shapes. We therefore extend *Shape* once more:

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

As we can see, all we need to do is to define our *Resize* `ModelAction` (followed by a colon (`:`) and the name `ModelAction`) and Appclusive knows we want a `ModelAction` in that model. Even better, now every model that *derives* from *Shape* also has that same `ModelAction`. 

But wait, something is missing: if we want to resize something, we have to know by what extend we want to resize it. So let's add another `ModelAttribute`, but this time *inside* the `ModelAction` directly:

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

Note: in order to force our users to supply a value for *NewSize* when calling the *Resize* `ModelAction` we mark the `ModelAttribute` as being `Required`. Appclusive will not process any *Resize* requests unless the user specifies a value for *NewSize*.

This is all good and fine but what does *NewSize* mean for a *Circle* or a *Square*? The answer to this is: *it depends*. For the *Circle* we decide to use *NewSize* as the new *Radius* and for the *Square* we decide to use *NewSize* as the new *Length*. 

This is something we call polymorphism. Both, *Circle* and *Square*, are a *Shape*, which means they have common ground. But in detail they might be more specialised and differ in their use and operation. But as long as we know how to resize a *Shape* (i.e. knowing the name of the `ModelAction` and its `ModelAttributes`) we will also know how to resize any other *derived* shape.

# Advanced Modelling

After happily playing around and resizing our shapes for a while we realise we would also like to have a new *Rhomboid* `Model` instead of only a *Square*. So what do we do?

We define a new `Model` that also derives from *Shape*:

``` csharp
public class Rhomboid : Shape
{
  [AttributeName("com.example.Rhomboid.Length1")]
  public float Length1 { get; set; }

  [AttributeName("com.example.Rhomboid.Length2")]
  public float Length2 { get; set; }
}
```

## Overriding ModelActions

We gave the *Rhomboid* two `ModelAttributes` *Length1* and *Length2* so our it can have two differing lengths. But what about our *Resize* `ModelAction`? It only has a single *NewSize* `ModelAttribute`. How do we resize both `ModelAttributes` of our *Rhomboid*? The answer is simple: We override the existing `ModelAction`:

``` csharp
public class Rhomboid : Shape
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

Here we see that we derived the existing `ModelAttribute` *NewSize* from the parent *Shape* as the Length1 parameter and added a new `ModelAttribute` *Length2*. The former attribute was marked as `Required` from its *Parent* and *Length2* is effectively optional (as it is not marked as `Required`). Our *Rhomboid* will then use the value *NewSize* for *Length1* and *Length2* if no "Length2* was specified by the user. The benefit of this is that any user who knows how to *Resize* a *Shape* still knows how to resize a *Rhomboid*.

And now we see that we can further re-arrange our existing *Square* as it has something in common with *Rhomboid*:

``` csharp
public class Square : Rhomboid
{
  public new class Resize : Rhomboid.Resize
  {
  }
}
```

Here we see that the definition of our *Square* has been simplified. We do not need any special *Length* property any more and the *Resize* `ModelAction` is directly taken from the *Rhomboid*. A user who knows a *Shape* and only supplies the *NewSize* `ModelAttribute` can resize a shape and a user who supplies the second `ModelAttribute` *Length2* from the *Rhomboid* `Model` can also resize a *Square* (of course we will have to inform the user if both of the lengths do not match as a *Square* must have all the same lengths).

By this we can see that we can simplify and re-use components if we design our `Models` carefully.

This example could be further extended by supporting a *Rectangle* as well (which would lead to yet another *Paren`* in our model chain).

# ModelStates

*WIP*

# Behaviours

*WIP* should be better described in a separate file
