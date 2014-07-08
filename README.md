QueryKit
========

[![Build Status](http://img.shields.io/travis/kylef/QueryKit/master.svg?style=flat)](https://travis-ci.org/kylef/QueryKit)

QueryKit, a simple CoreData query language for Swift.

## Usage

```swift
Person.queryset(context).filter(Person.name == "Kyle").delete()
```

### Querying

To retrieve objects from CoreData with QueryKit, you can construct a QuerySet
for your model in a managed object context.

A queryset is an immutable object, any operation will return a new queryset
instead of modifying the queryset.

```swift
var queryset = Person.queryset(context)
```

#### Filtering

You can filter a queryset using the `filter` and `exclude` methods, which
accept a predicate and return a new queryset.

```swift
queryset.filter(NSPredicate(format:"name == %@", "Kyle"))
queryset.filter(Person.name == "Kyle")
queryset.exclude(Person.age < 21)
queryset.exclude(Person.isEmployed)
```

#### Ordering

You can order a queryset's results by using the `orderBy` method which accepts
a collection of sort descriptors:

```swift
queryset.orderBy(NSSortDescriptor(key: "name", ascending: true))
queryset.orderBy(Person.name.ascending)
queryset.orderBy([Person.name.ascending, Person.age.descending])
```

#### Slicing

You can use slicing to limit a queryset to a range. For example, to get the
first 5 items:

```swift
queryset[0..5]
```

### Fetching

#### Single object

```swift
var kyle = queryset.filter(Person.name == "Kyle").get()
```

#### Object at index

```swift
var orta = queryset[3]
```

#### Count

```swift
println("There are \(queryset.count() - 1) more Kyle's.")
```

#### Iteration

```swift
for person in queryset {
    println(person.name)
}
```

#### Conversion to an array

```swift
queryset.array()
```

### Deleting

This method immediately deletes the objects in your queryset and returns a
count and an error if the operation failed.

```swift
queryset.delete()
```

## Predicate extensions

We've extended NSPredicate to add support for the `!`, `&&` and `||` operators
for joining predicates together.

```swift
NSPredicate(format:"name == Kyle") || NSPredicate(format:"name == Katie")
NSPredicate(format:"age >= 21") && !NSPredicate(format:"name == Kyle")
```

```swift
Person.name == "Kyle" || Person.name == "Katie"
Person.age >= 21 || Person.name != "Kyle"
```

## License

QueryKit is released under the BSD license. See [LICENSE](LICENSE).

