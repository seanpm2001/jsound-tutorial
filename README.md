# Getting started with JSound 2.0

This tutorial gives an introduction to JSound 2.0, starting with its compact syntax.

# JSound's type system

JSound 2.0 is a schema language that validates and annotates JSON documents.

Whereas some other JSON Schema languages are based on filtering JSON documents, a JSound schema is best seen as a collection of type declarations.

JSound supports four kinds of types corresponding to JSON's spirit:

- atomic types, with a selection of builtin, ready-to-use types: string, integer, date, hexBinary to name a few
- object types, to validate and annotate JSON objects
- array types, to validate and annotate JSON arrays
- union types, to combine several types and union their value spaces

# A hello world

Let us start with a simple example.

This is a JSON instance:

```
{
  "name" : "JSound"
}
```

Imagine we would like to validate such instances against a type, let us call it my-type, and say that for objects in that type's value space, the name field must be a string.

The type declaration itself, in JSound's comptact syntax, mimics the instance, like so:

```
{
  "name" : "string"
}
```

Now, if we want to put it inside a JSound schema and give the type a name, we associate this type declaration with the type name like so:

```
{
  "my-type" : {
    "name" : "string"
  }
}
```

Note that this type declaration does not prevent further fields from being present, which is the default behavior. Also, it does not require the field name to be present -- but if it is, it must be a string.

These are a few instances valid against my-type:

```
{ "name" : "James Kirk" }
{ "name" : "Beverly Crusher" }
{ "name" : "Spock", "origin" : "Vulcan", "age" : 234 }
{ "spaceship" : "USS Discovery", "name" : "Michael Burnham" }
{ }
{ "century" : 23 }
```

And a few that are not:
```
{ "name" : 1 }
{ "name" : null }
{ "name" : [ "Spock" ], "age" : 123 }
{ "bar" : 1, "name" : { "first" : "James", "last" : "Kirk" }
```

In order to make the name field required, we can add an exclamation mark, like so:

```
{
  "my-type" : {
    "name!" : "string"
  }
}
```

Then, these documents become invalid:

```
{ }
{ "century" : 23 }
```

We can also supply a default value, like so:

```
{
  "my-type" : {
    "name" : "string=N/A"
  }
}
```

Then the following instances would be valid:

```
{ }
{ "century" : 23 }
```

But an annotation step would actual produce a different output, populating default values:

```
{ "name" : "N/A" }
{ "century" : 23, "name" : "N/A"  }
```