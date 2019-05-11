# 

# Data structures

## 

## Map

### 

### Immutable map (default)

create a map

- val capitals = Map("Washington" -> "Olympia", "Massachusetts" -> "Boston", "California" -> "Sacramento")
- val capitals = Map(("Washington", "Olympia"), ("Massachusetts", "Boston"), ("California" ,"Sacramento"))

access keys

- capitals("Washington") // returns "Olympia" or throws exception if key doesn't exist
- capitals.getOrElse("Oregon", "Portland") // returns the default if key doesn't exist

check key existence

- capitals.contains("Washington")

add keys

- val newCapitals = capitals + ("Colorado" -> "Denver", "Ohio" -> "Columbus")

remove keys

- val newCapitals = capitals - ("Washington")

iterate over maps

- for ((key, value) <- map) process key and value

get all keys or values

- capitals.keys // returns Iterable
- capitals.keySet // returns an immutable set
- capitals.values // returns Iterable

### 

### Mutable map

create a map

- val capitals = scala.collection.mutable.Map("Washington" -> "Olympia", "Massachusetts" -> "Boston", "California" -> "Sacramento")

update keys or add new keys

- capitals("Washington") = "DC"
- capitals += ("Colorado" -> "Denver", "Ohio" -> "Columbus") // add multiple keys

remove keys

- capitals -= "Colorado"

### 

### Sorted map

create a map

- val capitals = scala.collection.immutable.SortedMap("Washington" -> "Olympia", "Massachusetts" -> "Boston", "California" -> "Sacramento")

### 

### map in insertion order

create a map

- val capitals = scala.collection.mutable.LinkedHashMap("Washington" -> "Olympia", "Massachusetts" -> "Boston", "California" -> "Sacramento")

## 

## Tuples

- val t = ("hello", 3.14, "world", true) // type: Tuple4[String, Double, String, Boolean]
- t._1 // returns "hello", 1-based index
- val (str1, f, str2, b) = t // pattern matching
- Array(1, 2, 3, 4).partition(_ > 2) // returns (Array(3,4), Array(1,2))

https://github.com/jduan/ScalaCheatsheet