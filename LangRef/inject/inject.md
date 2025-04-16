
## [Inject Method: Explained](https://medium.com/@terrancekoar/inject-method-explained-ed531eff9af8)

When called, the inject method will pass each element and accumulate each sequentially.
```
[3, 6, 10, 13].inject(:+) => (((3 + 6) + 10) + 13) => 32
  ':+' symbol signifying the arithmetic operation i.e :+, :-, :*, :/, etc.

or 

[3, 6, 10].inject {|sum, number| sum + number} =>|3, 6| 3 + 6 => 9
                                               =>|9, 10| 9 + 10 =>19

```

To break this down even further, inject takes the first element in your collection and uses that as the base ‘sum’. It then takes the next element (or the second element in the array) and then adds them together. The method then assigns that result to the ‘sum’ and adds in the next element in the collection until all elements have been passed through the block. In this case, ‘sum’ is what we call an accumulator — as it is accumulating the values. The return value will be the sum of all the elements in the collection.

### set starting value
```
# pass the starting value for the accumulator i.e. 0
[3, 6, 10, 13].inject(0, :+) => 32
[3, 6, 10].inject(0) {|sum, number| sum + number} => 19

```
Here, you are giving the method a starting value for the accumulator, since I passed it zero, the method will add the first element of the collection to zero before moving on to the addition of the second and third elements in the collection.


### building hashes
```
[[:student, "Terrance Koar"], [:course, "Web Dev"]].inject({}) do |result, element| 
    result[element.first] = element.last 
    result
end
#=> {:student=>"Terrance Koar", :course=>"Web Dev"}

```

### convert data types

Additionally, inject can be used to convert data types with the help of some other cool methods — even while it’s building a new hash!

```
[[:student, "Terrance Koar"], [:course, "Web Dev"]].inject({}) do |result, element| 
    result[element.first.to_s] = element.last.upcase
    result
end
# => {"student"=>"TERRANCE KOAR", "course"=>"WEB DEV"}

OR

[[:student, "Terrance Koar"], [:course, "Web Dev"]].inject({}) do |result, element| 
    result[element.first.to_s] = element.last.split
    result
end
# => {"student"=>["Terrance", "Koar"], "course"=>["Web", "Dev"]}

```

### filter
```
[10, 20, 30, 5, 7, 9, 3].inject([]) do |result, element| 
     result << element.to_s if element > 9
     result
end
# => ["10", "20", "30"]

```




