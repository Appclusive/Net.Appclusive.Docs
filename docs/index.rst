.. highlight:: csharp
.. role:: csharp(code)
    :language: csharp

=================
This is a heading
=================

=====  =====  =======
A      B      A and B
=====  =====  =======
False  False  False
True   False  False
False  True   False
True   True   True
=====  =====  =======

| These lines are
| broken exactly like in
| the source file.



term (up to a line of text)
   Definition of the term, which must be indented

   and can even consist of multiple paragraphs

next term
   Description.
   
This is a normal text paragraph. The next paragraph is a code sample

::

   It is not processed in any way, except
   that the indentation is removed.

   It can span multiple lines.

This is a normal text paragraph again.


Here are some tests:

* Item1
* Item2
* Item3

A `Behaviour` is something we should really understand, as it is the *basis* for alle `Models`. But the main question is: **How do we quote code for a specific language, such as CSharp?**.

Some paragraph

	Some other paragraph...

According to [some StackOverflow article](http://stackoverflow.com/questions/10870719/inline-code-highlighting-in-restructuredtext) code highlighting is easy :code:`var n = 42L`.

Block code is formatted as this example here

::

	namespace Net.Appclusive.Public.Model.Inventory
	{
		public class Behaviour : BaseEntity
		{
			[JsonIgnore]
			public virtual ICollection<Behaviour> Children { get; set; }

			[JsonIgnore]
			public virtual ICollection<Behaviour> Parents { get; set; }

			[JsonIgnore]
			public virtual ICollection<Model> Models { get; set; }
		}
	}

Tralala, and now we are the end, my friend ...
