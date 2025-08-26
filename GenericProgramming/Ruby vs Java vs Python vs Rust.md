# Collections

## Enumerable

### all?

1.  Java

    ``` java
    Stream.of(1, 2, 3, 4, 5).allMatch(x -> x < 10);
    ```

2.  Python

    ``` python
    all(x < 10 for x in [1, 2, 3, 4, 5])
    ```

3.  Rust

    ``` rust
    let x = vec![1, 2, 3, 4];
    let y = x.iter().all(|e| *e < 10);
    ```

4.  Swift

    ``` swift
    let x = [1, 2, 3, 4]
    let y = x.allSatisfy { $0 < 10 }
    ```

5.  Ruby

    ``` ruby
    [1, 2, 3, 4, 5].all? { |x| x < 10 }
    ```

### any?

1.  Java

    ``` java
    Stream.of(1, 2, 3, 4, 5).anyMatch(x -> x < 10);
    ```

2.  Python

    ``` python
    any(x < 10 for x in [1, 2, 3, 4, 5])
    ```

3.  Rust

    ``` rust
    let x = vec![1, 2, 3, 4];
    let y = x.iter().any(|e| *e < 10);
    ```

4.  Swift

    ``` swift
    let x = [1, 2, 3, 4]
    let y = x.contains { $0 < 10 }
    ```

5.  Ruby

    ``` ruby
    [1, 2, 3, 4, 5].any? { |x| x < 10 }
    ```

### collect / map

1.  Java

    ``` java
    Stream.of(1, 2, 3, 4).map(x -> x * 2);
    ```

2.  Python

    ``` python
    [e * 2 for e in [1, 2, 3, 4]]
    ```

3.  Rust

    ``` rust
    let x = vec![1, 2, 3, 4];
    let y: Vec<i32> = x.iter().map(|x| x * 2).collect();
    ```

4.  Swift

    ``` swift
    let x = [1, 2, 3, 4]
    let y = x.map { $0 * 2 }
    ```

5.  Ruby

    ``` ruby
    [1, 2, 3, 4].map { |x| x * 2 }
    # 或
    [1, 2, 3, 4].collect { |x| x * 2 }
    ```

### `flat_map`

1.  Java

    ``` java
    List a = Arrays.asList(1, 2, 3, 4);
    List b = Arrays.asList(3, 4, 5, 6);
    List result = Stream.of(a, b).flatMap(e -> e.stream()).collect(Collectors.toList());
    ```

2.  Python

    ``` python
    x = [[1, 2, 3], [4, 5, 6]]

    [e for i in x for e in i]
    ```

3.  Rust

    ``` rust
    let x = vec![
        vec![1, 2, 3],
        vec![4, 5, 6]
    ];
    let y: Vec<&i32> = x.iter().flat_map(|e| e ).collect();
    ```

4.  Swift

    ``` swift
    let x = [[1, 2, 3], [4, 5, 6]]
    let y = x.flatMap { $0 }
    ```

5.  Ruby

    ``` ruby
    x = [[1, 2, 3], [4, 5, 6]]
    x.flat_map { |e| e }
    # 或
    x.flat_map(&:itself)
    ```

### count

1.  Java

    ``` java
    IntStream.range(1, 11).filter(e -> e % 2 == 0).count()
    ```

2.  Python

    ``` python
    sum(1 for e in [1, 2, 3, 4] if e < 3)
    ```

3.  Rust

    ``` rust
    let x = vec![1, 2, 3, 4];
    let y = x.iter().filter(|e| **e < 3).count();
    ```

4.  Swift

    ``` swift
    let x = [1, 2, 3, 4]
    let y = x.count { $0 < 3 }
    ```

5.  Ruby

    ``` ruby
    [1, 2, 3, 4].count { |e| e < 3 }
    ```

### find

1.  Java

    ``` java
    Integer i = Stream.of(1, 2, 3, 4).filter(x -> x > 2).findFirst();
    ```

2.  Python

    ``` python
    next((e for e in [1, 2, 3, 4, 5, 6] if e > 3), None)
    ```

3.  Rust

    ``` rust
    let x = vec![1, 2, 3, 4];
    let y = x.iter().find(|e| **e > 3);
    ```

4.  Swift

    ``` swift
    let x = [1, 2, 3, 4]
    let y = x.first { $0 > 3 }
    ```

5.  Ruby

    ``` ruby
    [1, 2, 3, 4, 5, 6].find { |e| e > 3 }
    # 或
    [1, 2, 3, 4, 5, 6].detect { |e| e > 3 }
    ```

### `each_with_index`

1.  Java

    ``` java
    // Java 没有对应的现成方法
    // Method A:
    String[] names = {"Sam", "Pamela", "Dave", "Pascal", "Erik"};
    IntStream.range(0, names.length)
        .filter(i -> names[i].length() <= i)
        .mapToObj(i -> names[i])
        .collect(Collectors.toList());

    // Method B:
    String[] names = {"Sam", "Pamela", "Dave", "Pascal", "Erik"};
    AtomicInteger index = new AtomicInteger();
    List<String> list = Arrays.stream(names)
        .filter(n -> n.length() <= index.incrementAndGet())
        .collect(Collectors.toList());

    // Method C:
    // Guava since version 21
    Streams.mapWithIndex(Stream.of("a", "b", "c"),
                         (str, index) -> str + ":" + index) // will return Stream.of("a:0", "b:1", "c:2")
    ```

2.  Python

    ``` python
    for i, v in enumerate(a):
       print "{} : {}".format(i, v)
    ```

3.  Rust

    ``` rust
    let x = vec![1, 2, 3, 4];
    x.iter().enumerate().for_each(|(idx, e)| {
        println!("Index: {}, Value: {}", idx, e);
    });
    ```

4.  Swift

    ``` swift
    let x = [1, 2, 3, 4]
    x.enumerated().forEach { (index, element) in
        print("\(index): \(element)")
    }
    ```

5.  Ruby

    ``` ruby
    x = [1, 2, 3, 4]
    x.each_with_index { |e, i| puts "#{i}: #{e}" }
    ```

### `each_with_object`

1.  Java

    ``` ruby
    h = {a: 1, b: 2, c: 3}
    result = h.each_with_object({}) { |(k, v), hash| hash[k] = v }
    ```

    ``` java
    Map<String, Integer> h = ImmutableMap.of("a", 1, "b", 2, "c", 3);
    Map<String, Integer> result = h.entrySets().stream().reduce(new HashMap<String, Integer>(), (hash, es) -> {
            hash.put(es.getKey(), es.getValue() * 2);
            return hash;
        }, (h1, h2) -> {
            h1.putAll(h2);
            return h1;
        });
    ```

2.  Python

    ``` python
    def create_url_table(urls):
      return {url: get_title(url) for url in urls if 'google' not in url}
    ```

3.  Rust

    ``` rust
    let x = vec![1, 2, 3, 4];
    let y = x.iter().fold(HashMap::new(), |mut acc, &x| {
        *acc.entry(x).or_insert(0) += 1;
        acc
    });
    ```

4.  swift

    ``` swift
    let x = [1, 2, 3, 4]
    let y = x.reduce(into: [:]) { $0[$1, default: 0] += 1 }
    ```

5.  Ruby

    ``` ruby
    h = { a: 1, b: 2, c: 3 }
    result = h.each_with_object({}) { |(k, v), hash| hash[k] = v }
    # 计数示例
    x = [1, 2, 3, 4, 3, 2]
    counts = x.each_with_object(Hash.new(0)) { |n, acc| acc[n] += 1 }
    ```

### entries

1.  Java

    ``` example
    Map#entrySet()
    ```

2.  Python

    ``` python
    d.items()  # Or: list(d.items())
    ```

3.  Rust

    ``` rust
    let x: HashMap<String, i32> = HashMap::new();
    let entries: Vec<_> = x.iter().collect_vec();
    ```

4.  swift

    ``` swift
    Array(dict).forEach { (key, value) in
        print("\(key): \(value)")
    }
    ```

5.  Ruby

    ``` ruby
    h = { a: 1, b: 2 }
    h.entries # => [[:a, 1], [:b, 2]]
    # 或
    h.to_a
    ```

### `find_all` / select / filter

1.  Java

    ``` example
    Stream#filter()
    ```

2.  Python

    ``` python
    [e for e in [1, 2, 3, 4] if e > 2]
    ```

3.  Rust

    ``` rust
    let x = vec![1, 2, 3, 4];
    let x = x.iter().filter(|e| **e > 2).collect_vec();
    ```

4.  Swift

    ``` swift
    let x = [1, 2, 3, 4]
    let y = x.filter { $0 > 2 }
    ```

5.  Ruby

    ``` ruby
    [1, 2, 3, 4].select { |e| e > 2 }
    # 或
    [1, 2, 3, 4].find_all { |e| e > 2 }
    ```

### `group_by`

1.  Java

    ``` java
    Map<Boolean, List<Integer>> map = IntStream.range(1, 11).boxed().collect(groupingBy(x -> x % 2 == 0));
    ```

2.  Python

    ``` python
    from itertools import groupby
    from operator import itemgetter

    x = [{"name": "Jack", "gender": "M"}, {"name": "Lucy", "gender": "F"}, {"name": "David", "gender": "M"}]

    h = {k: list(v) for k, v in  groupby(sorted(x, key=itemgetter('gender')), key=itemgetter('gender')) }

    # Out[153]: 
    # {'F': [{'name': 'Lucy', 'gender': 'F'}],
    #  'M': [{'name': 'Jack', 'gender': 'M'}, {'name': 'David', 'gender': 'M'}]}
    ```

3.  Rust

    ``` rust
    use itertools::Itertools;

    // group data into runs of larger than zero or not.
    let data = vec![1, 3, -2, -2, 1, 0, 1, 2];
    // groups:     |---->|------>|--------->|

    for (key, group) in data.into_iter().group_by(|elt| *elt >= 0) {
        // Check that the sum of each group is +/- 4.
        assert_eq!(4, group.iter().fold(0_i32, |a, b| a + b).abs());
    }
    ```

4.  swift

    ``` swift
    let x = [1, 2, 3, 4]
    let y = Dictionary(grouping: x, by: { $0 % 2 == 0 })
    ```

5.  Ruby

    ``` ruby
    x = [1, 2, 3, 4]
    x.group_by { |e| e.even? }
    # => {false=>[1, 3], true=>[2, 4]}
    ```

### inject / reduce

1.  Java

    ``` java
    IntStream.range(1, 11).boxed().reduce(Integer::sum);
    ```

2.  Python

    ``` python
    reduce(function, iterable[, initializer])
    ```

3.  Rust

    ``` rust
    let x = vec![1, 2, 3, 4];
    let s1 = x.iter().fold(0, |s, e| s + *e);
    ```

4.  Swift

    ``` swift
    let x = [1, 2, 3, 4]
    let y = x.reduce(0, +)
    ```

5.  Ruby

    ``` ruby
    [1, 2, 3, 4].inject(0, :+)
    # 或
    [1, 2, 3, 4].reduce(0) { |s, e| s + e }
    ```

### map

1.  Java

    ``` java
    Stream map = IntStream.range(1, 11).boxed().map(x -> x * 2);
    ```

2.  Python

    ``` python
    [e * 2 for e in [1, 2, 3, 4, 5]]
    ```

3.  Rust

    ``` rust
    let x = vec![1, 2, 3, 4];
    let s = x.iter().map(|x| *x + 2).collect_vec();
    ```

4.  Swift

    ``` swift
    let x = [1, 2, 3, 4]
    let y = x.map { $0 * 2 }
    ```

5.  Ruby

    ``` ruby
    [1, 2, 3, 4].map { |x| x * 2 }
    ```

### max / `max_by`

1.  Java

    ``` java
    IntStream.range(1, 11).max();
    IntStream.range(1, 11).boxed().max(Comparators.comparable());
    IntStream.range(1, 11).boxed().max(Comparator.comparing(x -> -x));
    IntStream.range(1, 11).boxed().collect(maxBy(Comparator.comparing(x -> x)));
    ```

2.  Python

    ``` python
    max(e for e in [1, 2, 3, 4])
    max((e for e in [1, 2, 3, 4]), key=lambda e: -e)
    ```

3.  Rust

    ``` rust
    let x = vec![1, 2, 3, 4];
    let m = x.iter().max();
    let m2 = x.iter().max_by_key(|e| -(**e) );
    let m3 = x.iter().max_by(|a, b| (*b).cmp(*a));
    ```

4.  swift

    ``` swift
    let x = [1, 2, 3, 4]
    let y = x.max { $0 < $1 }
    ```

5.  Ruby

    ``` ruby
    [1, 2, 3, 4].max
    [1, 2, 3, 4].max_by { |e| -e }
    ```

### partition

1.  Java

    ``` java
    IntStream.range(1, 11).boxed().collect(partitioningBy(x -> x > 5))
    ```

2.  Python

    ``` python
    def partition(pred, iterable):
        t1, t2 = itertools.tee(iterable)
        return itertools.filterfalse(pred, t1), itertools.filter(pred, t2)
    ```

3.  Rust

    ``` rust
    let x = vec![1, 2, 3, 4];
    let (x, y): (Vec<i32>, Vec<i32>) = x.iter().partition(|e| **e > 2);
    ```

4.  swift

    ``` swift
    var idx = names.partition { $0.count > 4 }
    var first = names[..<idx]
    var second = names[idx...]
    ```

5.  Ruby

    ``` ruby
    [1, 2, 3, 4].partition { |e| e > 2 }
    # => [[3, 4], [1, 2]]
    ```

### `reverse_each`

1.  Java

    ``` java
    IntStream.range(1, 11).boxed().sorted(Collections.reverseOrder()).forEach(System.out::println);
    ```

2.  Python

    ``` python
    for i in reversed([1, 2, 3, 4]):
        print(i)
    ```

3.  Rust

    ``` rust
    let x = vec![1, 2, 3, 4];
    x.iter().rev().for_each(|e| println!("{}", e));
    ```

4.  swift

    ``` swift
    let x = [1, 2, 3, 4]
    x.reversed().forEach { print($0) }
    ```

5.  Ruby

    ``` ruby
    [1, 2, 3, 4].reverse_each { |e| puts e }
    ```

### sort

1.  Java

    ``` java
    List l = Arrays.asList(4, 3, 2, 1);
    l.sort(Comparator.naturalOrder());
    Collections.sort(l);
    System.out.println(l);
    l.stream().sorted();
    l.stream().sorted(Comparators.comparable());
    l.stream().sorted(Comparator.naturalOrder());
    ```

2.  Python

    ``` python
    sorted([3, 1, 4, 2])
    ```

3.  Rust

    ``` rust
    let x = vec![2, 3, 1, 4];
    let x = x.iter().sorted().collect_vec();
    ```

4.  swift

    ``` swift
    let x = [2, 3, 1, 4]
    let y = x.sorted()
    ```

5.  Ruby

    ``` ruby
    [2, 3, 1, 4].sort
    ```

### `sort_by`

1.  Java

    ``` java
    people.stream().sorted(Comparator.comparing(x -> x.getAge()))
    ```

2.  Python

    ``` python
    from operator import itemgetter
    h = [{"name": "Jack", "age": 20}, {"name": "David", "age": 30}, {"name": "Lucy", "age": 10}]
    sorted(h, key=itemgetter("age"))
    ```

3.  Rust

    ``` rust
    let x = vec![
        Person { name: String::from("Jack"), age: 20 },
        Person { name: String::from("David"), age: 10 },
        Person { name: String::from("Lucy"), age: 30 },
    ];

    let x = x.iter().sorted_by_key(|p| &p.name).collect_vec();
    ```

4.  swift

    ``` swift
    let x = [2, 3, 1, 4]
    let y = x.sorted { $0 < $1 }
    ```

5.  Ruby

    ``` ruby
    people = [
      { name: "Jack", age: 20 },
      { name: "David", age: 10 },
      { name: "Lucy", age: 30 }
    ]
    people.sort_by { |p| p[:age] }
    ```

### sum

1.  Java

    ``` java
    IntStream.range(1, 11).sum();
    Stream.of(1, 2, 3, 4).mapToInt(Integer::intValue).summaryStatistics().getSum();
    ```

2.  Python

    ``` python
    sum([1, 2, 3, 4])
    ```

3.  Rust

    ``` rust
    let y = vec![2, 3, 4, 5];
    let y = y.iter().sum();
    ```

4.  swift

    ``` swift
    let x = [1, 2, 3, 4]
    let y = x.reduce(0, +)
    ```

5.  Ruby

    ``` ruby
    [1, 2, 3, 4].sum
    # 早期 Ruby 可使用 inject:
    [1, 2, 3, 4].inject(0, :+)
    ```

### uniq

1.  Java

    ``` java
    Stream.of(1, 2, 3, 1, 2, 3).distinct().forEach(System.out::println);
    ```

2.  Python

    ``` python
    list(set([1, 2, 3, 4, 1, 2]))
    ```

3.  Rust

    ``` rust
    let y = vec![2, 3, 4, 5];
    let y = y.iter().unique().collect_vec();
    ```

4.  swift

    ``` swift
    let x = [1, 2, 3, 1, 2, 3]
    let y = Array(Set(x))

    import Algorithm
    let y = x.uniqued()
    ```

5.  Ruby

    ``` ruby
    [1, 2, 3, 1, 2, 3].uniq
    ```

### zip

1.  Java

    Guava:

    ``` java
    Streams.zip(
                Stream.of("foo1", "foo2", "foo3"),
                Stream.of("bar1", "bar2"),
                (arg1, arg2) -> arg1 + ":" + arg2);
    ```

2.  Python

    ``` python
    x = [1, 2, 3]
    y = [4, 5, 6]
    zipped = zip(x, y)
    ```

3.  Rust

    ``` rust
    let a1 = [1, 2, 3];
    let a2 = [4, 5, 6];
    let mut iter = a1.iter().zip(a2.iter());
    ```

4.  swift

    ``` swift
    let x = [1, 2, 3]
    let y = [4, 5, 6]
    let zipped = zip(x, y)
    ```

5.  Ruby

    ``` ruby
    [1, 2, 3].zip([4, 5, 6])
    # => [[1, 4], [2, 5], [3, 6]]
    ```

## Array

### &

1.  Java

    commons-collections `ListUtils` :

    ``` java
    public static List intersection(List list1, List list2)
    ```

2.  Python

    ``` python
    a = [1,2,3,4,5]
    b = [1,3,5,6]
    list(set(a) & set(b))

    [x for x in a if x in b]
    ```

3.  Rust

    ``` rust
    use std::collections::HashSet;

    let a: HashSet<u32> = vec![1, 2, 3].into_iter().collect();
    let b: HashSet<_> = [4, 2, 3, 4].iter().cloned().collect();

    let mut intersection = a.intersection(&b);
    ```

4.  swift

    ``` swift
    let a = Set([1, 2, 3, 4, 5])
    let b = Set([1, 3, 5, 6])
    let c = a.intersection(b)
    ```

5.  Ruby

    ``` ruby
    [1, 2, 3, 4, 5] & [1, 3, 5, 6]
    ```

### \|

1.  Java

    commons-collection `ListUtils#union()`

    Returns a new list containing the second list appended to the first
    list.

    Result may be not unique.

2.  Python

    ``` python
    a = ['Orange and Banana', 'Orange Banana']
    b = ['Grapes', 'Orange Banana']
    c = ['Foobanana', 'Orange and Banana']
    list(set().union(a,b,c))
    ```

3.  Rust

    ``` rust
    use std::collections::HashSet;

    let a: HashSet<u32> = vec![1, 2, 3].into_iter().collect();
    let b: HashSet<_> = [4, 2, 3, 4].iter().cloned().collect();

    let mut union_iter = a.union(&b);
    ```

4.  swift

    ``` swift
    let a = Set([1, 2, 3, 4, 5])
    let b = Set([1, 3, 5, 6])
    let c = a.union(b)
    ```

5.  Ruby

    ``` ruby
    [1, 2] | [2, 3]
    ```

### +

1.  Java

    ``` java
    Stream.concat(stream1, stream2);
    List#addAll();
    ListUtils.union(l1, l2);
    ```

2.  Python

    ``` python
    [1, 2] + [3, 4]
    ```

3.  Rust

    ``` rust
    let y = vec![2, 3, 4, 5];
    let y = y.iter().chain(y.iter()).collect_vec();
    ```

4.  swift

    ``` swift
    let x = [1, 2]
    let y = [3, 4]
    let z = x + y
    ```

5.  Ruby

    ``` ruby
    [1, 2] + [3, 4]
    ```

### -

1.  Java

    ``` java
    boolean result = List#removeAll();
    List result = ListUtils.removeAll(col, removeCol); // commons-collections
    ```

2.  Python

    ``` python
    set([1,2,3,4]) - set([2,5])
    ```

3.  Rust

    ``` rust
    use std::collections::HashSet;

    let a: HashSet<u32> = vec![1, 2, 3].into_iter().collect();
    let b: HashSet<_> = [4, 2, 3, 4].iter().cloned().collect();

    let mut difference = a.difference(&b);
    ```

4.  swift

    ``` swift
    let a = Set([1, 2, 3, 4, 5])
    let b = Set([2, 5])
    let c = a.subtracting(b)
    ```

5.  Ruby

    ``` ruby
    [1, 2, 3, 4, 5] - [2, 5]
    ```

### \<\<

1.  Java

    ``` java
    List#add()
    ```

2.  Python

    ``` python
    x = [1, 2, 3]
    x.append(4)
    ```

3.  Rust

    ``` rust
    let mut y = vec![2, 3, 4, 5];
    y.push(10);
    ```

4.  swift

    ``` swift
    var x = [1, 2, 3]
    x.append(4)
    ```

5.  Ruby

    ``` ruby
    x = [1, 2, 3]
    x << 4
    x.push(5)
    ```

### compact

1.  Java

    ``` java
    boolean result = List#removeAll(Collections.singleton(null));
    List result = ListUtils.removeAll(col, Collections.singleton(null)); // commons-collections
    ```

2.  Python

    ``` python
    [e for e in x if e]
    ```

3.  Rust

    无

4.  swift

    ``` swift
    let x = [1, 2, nil, 3, 4]
    let y = x.compactMap { $0 }
    ```

5.  Ruby

    ``` ruby
    [1, 2, nil, 3, 4].compact
    ```

### delete

1.  Java

    see `compact`

2.  Python

    ``` python
    x = [3, 4, 5, 6]
    x.remove(3)
    ```

3.  Rust

    ``` rust
    let mut y = vec![2, 3, 4, 5];
    y.remove(1);  // remove by index
    ```

4.  swift

    ``` swift
    var x = [1, 2, 3, 4]
    x.remove(at: 1) // remove by index
    x.removeAll { $0 == 3 } // remove by value
    ```

5.  Ruby

    ``` ruby
    x = [3, 4, 5, 6]
    x.delete(3)    # 按值删除
    x.delete_at(1) # 按索引删除
    ```

### flatten

1.  Java

    ``` java
    List<List<Object>> list = ...
        List<Object> flat = list.stream()
            .flatMap(List::stream)
            .collect(Collectors.toList());
    ```

2.  Python

    ``` python
    flat_list = [item for sublist in t for item in sublist]
    ```

3.  Rust

    ``` rust
    let data = vec![vec![1, 2, 3, 4], vec![5, 6]];
    let flattened = data.into_iter().flatten().collect::<Vec<u8>>();
    assert_eq!(flattened, &[1, 2, 3, 4, 5, 6]);
    ```

4.  swift

    ``` swift
    let x = [[1, 2, 3], [4, 5, 6]]
    let y = x.flatMap { $0 }
    ```

5.  Ruby

    ``` ruby
    [[1, 2, 3], [4, 5, 6]].flatten
    # 指定层级：
    [[1, [2]], [3]].flatten(1)
    ```

### include?

1.  Java

    ``` java
    Collection#contains();
    Stream#anyMatch();
    ```

2.  Python

    ``` python
    2 in [2, 3, 4, 5]
    ```

3.  Rust

    ``` rust
    let mut y = vec![2, 3, 4, 5];
    y.contains(&2);
    ```

4.  swift

    ``` swift
    let x = [1, 2, 3, 4]
    let y = x.contains(2)
    ```

5.  Ruby

    ``` ruby
    [1, 2, 3, 4].include?(2)
    ```

### join

1.  Java

    ``` java
    String#join();
    String result = artists.stream().map(Artist::getName).collect(joining(",", "[", "]"));
    ```

2.  Python

    ``` python
    "XX".join([str(e) for e in x])
    ```

3.  Rust

    ``` rust
    let mut y = vec![2, 3, 4, 5];
    let y: String = y.iter().map(|e| (*e).to_string() ).join(",");
    ```

4.  swift

    ``` swift
    let x = [1, 2, 3, 4]
    let y = x.map { String($0) }.joined(separator: ",")
    ```

5.  Ruby

    ``` ruby
    [1, 2, 3, 4].join(",")
    ```

### last

1.  Java

    ``` java
    // Guava
    lastElement = Iterables.getLast(iterableList);
    // You can also provide a default value if the list is empty, instead of an exception:
    lastElement = Iterables.getLast(iterableList, null);

    stream.skip(count - 1).findFirst().get();
    ```

2.  Python

    ``` python
    x[-1]
    ```

3.  Rust

    ``` rust
    let mut y = vec![2, 3, 4, 5];
    println!("{:?}", y.last().unwrap());
    ```

4.  swift

    ``` swift
    let x = [1, 2, 3, 4]
    let y = x.last
    ```

5.  Ruby

    ``` ruby
    [1, 2, 3, 4].last
    ```

### first

1.  Java

    ``` java
    String firstElement = Iterables.getFirst(strings, null); // Guava

    Stream#findfirst();
    ```

2.  Python

    ``` python
    x[0]
    ```

3.  Rust

    ``` rust
    let mut y = vec![2, 3, 4, 5];
    println!("{:?}", y.first().unwrap());
    ```

4.  swift

    ``` swift
    let x = [1, 2, 3, 4]
    let y = x.first
    ```

5.  Ruby

    ``` ruby
    [1, 2, 3, 4].first
    ```

### push / pop / shift / unshift

1.  Java

    ``` java
    // push
    List#add();
    Stack#push();
    LinkedList#addLast();

    // pop
    list.remove(list.size-1);
    Stack#pop();
    LinkedList#pollLast();

    // shift
    list.remove(0);
    LinkedList#pollFirst();

    // unshift
    list.add(0, obj);
    LinkedList#addFirst();
    ```

2.  Python

    use `deque`

3.  Rust

    use `VecDeque`

4.  swift

    ``` swift
    var x = [1, 2, 3, 4]
    x.append(5) // push
    x.removeLast() // pop
    x.removeFirst() // shift
    x.insert(0, 0) // unshift
    ```

5.  Ruby

    ``` ruby
    x = [1, 2, 3, 4]
    x.push(5)     # push
    x.pop         # pop
    x.shift       # shift
    x.unshift(0)  # unshift
    ```

### reverse

1.  Java

    ``` java
    Collections.reverse();
    ArrayUtils.reverse(); // commons-lang
    IntStream.range(1, 11).boxed().sorted(Collections.reverseOrder()).forEach(System.out::println);
    ```

2.  Python

    ``` python
    list(reversed([1, 2, 3, 4]))
    ```

3.  Rust

    ``` rust
    let mut y = vec![2, 3, 4, 5];
    y.reverse();
    let y = y.iter().rev().collect_vec();
    ```

4.  swift

    ``` swift
    var x = [1, 2, 3, 4]
    x.reverse()  // 原地修改
    var y = x.reversed()  // 返回一个新的数组
    ```

5.  Ruby

    ``` ruby
    [1, 2, 3, 4].reverse      # 返回新数组
    x = [1, 2, 3, 4]; x.reverse!  # 原地修改
    ```

### rotate

1.  Java

    ``` java
    Collections.rotate();
    ```

2.  Python

    无

3.  Rust

    ``` rust
    let mut y = vec![2, 3, 4, 5];
    y.reverse();
    ```

4.  swift

    无

5.  Ruby

    ``` ruby
    [1, 2, 3, 4].rotate      # => [2, 3, 4, 1]
    [1, 2, 3, 4].rotate(2)   # => [3, 4, 1, 2]
    ```

### shuffle

1.  Java

    ``` java
    Collections.shuffle();
    ```

2.  Python

    ``` python
    random.shuffle(x)
    ```

3.  Rust

    ``` rust
    use rand::thread_rng;
    use rand::seq::SliceRandom;

    fn main() {
        let mut vec: Vec<u32> = (0..10).collect();
        vec.shuffle(&mut thread_rng());
        println!("{:?}", vec);
    }
    ```

4.  swift

    ``` swift
    var x = [1, 2, 3, 4]
    x.shuffle()  // 原地修改
    var y = x.shuffled()  // 返回一个新的数组
    ```

5.  Ruby

    ``` ruby
    x = [1, 2, 3, 4]
    x.shuffle   # 返回新数组
    x.shuffle!  # 原地修改
    ```

### sample

1.  Java

    无

2.  Python

    ``` python
    random.sample([1, 2, 3, 4, 5], 3)
    ```

3.  swift

    ``` swift
    let x = [1, 2, 3, 4, 5]
    let y = x.shuffled().prefix(3)
    ```

4.  Ruby

    ``` ruby
    [1, 2, 3, 4, 5].sample(3)
    # 单个元素：
    [1, 2, 3, 4, 5].sample
    ```

## Hash

### `each_key` / `each_value`

1.  Java

    ``` java
    Map#keySet();
    Map#values()
    ```

2.  Python

    ``` python
    h = {'name': 'Jack', 'age': 1}
    h.keys()
    h.values()
    ```

3.  Rust

    ``` rust
    use std::collections::HashMap;

    let mut map = HashMap::new();
    map.insert("a", 1);
    map.insert("b", 2);
    map.insert("c", 3);

    for key in map.keys() {
        println!("{}", key);
    }
    ```

4.  swift

    ``` swift
    let x = ["name": "Jack", "age": 1]
    x.keys.forEach { print($0) }
    x.values.forEach { print($0) }
    ```

5.  Ruby

    ``` ruby
    h = { name: 'Jack', age: 1 }
    h.each_key { |k| puts k }
    h.each_value { |v| puts v }
    # 或：
    h.keys  # => [:name, :age]
    h.values # => ['Jack', 1]
    ```

### invert

1.  Java

    ``` java
    MapUtils.invertMap(); // commons-collections
    ```

2.  Python

    ``` python
    inv_map = {v: k for k, v in my_map.items()}
    ```

3.  Rust

    无

4.  swift

    ``` swift
    let x = ["name": "Jack", "age": 1]
    let y = Dictionary(uniqueKeysWithValues: x.map { ($1, $0) })
    ```

5.  Ruby

    ``` ruby
    { name: 'Jack', age: 1 }.invert
    # => {"Jack"=>:name, 1=>:age}
    ```

### merge

1.  Java

    ``` java
    Map#pullAll()
    ```

2.  Python

    ``` python
    z = x | y  # 3.9+
    z = {**x, **y}  # 3.5+
    ```

3.  Rust

    ``` rust
    use std::collections::HashMap;

    // Mutating one map
    fn merge1(map1: &mut HashMap<(), ()>, map2: HashMap<(), ()>) {
        map1.extend(map2);
    }

    // Without mutation
    fn merge2(map1: HashMap<(), ()>, map2: HashMap<(), ()>) -> HashMap<(), ()> {
        map1.into_iter().chain(map2).collect()
    }

    // If you only have a reference to the map to be merged in
    fn merge_from_ref(map: &mut HashMap<(), ()>, map_ref: &HashMap<(), ()>) {
        map.extend(map_ref.into_iter().map(|(k, v)| (k.clone(), v.clone())));
    }

    fn main() {
        println!("Hello, world!");
    }
    ```

4.  swift

    ``` swift
    let x = ["name": "Jack", "age": 1]
    let y = ["name": "David", "age": 2]
    let z = x.merging(y) { $1 }
    ```

5.  Ruby

    ``` ruby
    x = { name: 'Jack', age: 1 }
    y = { name: 'David', age: 2 }
    x.merge(y)            # 非破坏性
    x.merge!(y)           # 破坏性
    # 自定义冲突解决：
    x.merge(y) { |_k, old, newv| newv }
    ```

### `transform_keys` / `transform_values`

1.  Java

    ``` java
    // Guava
    Maps.transformEntries();
    Maps.transformValues();
    ```

2.  Python

    ``` python
    alphabet =  {k.lower(): v for k, v in alphabet.items()}
    ```

3.  Rust

    无

4.  swift

    无

5.  Ruby

    ``` ruby
    h = { 'Name' => 'Jack', 'Age' => 1 }
    h.transform_keys(&:downcase)      # => {"name"=>"Jack", "age"=>1}
    h.transform_values { |v| v.to_s } # => {"Name"=>"Jack", "Age"=>"1"}
    # 非破坏性版本；破坏性：transform_keys! / transform_values!
    ```

### values

1.  Java

    ``` java
    Map#values()
    ```

2.  Python

    ``` python
    h.values()
    ```

3.  Rust

    ``` rust
    map.values()
    ```

4.  swift

    ``` swift
    let x = ["name": "Jack", "age": 1]
    let y = x.values
    ```

5.  Ruby

    ``` ruby
    { name: 'Jack', age: 1 }.values
    ```

### `values_at`

1.  Java

    ``` java
    Maps#filterKeys() // Guava
    ```

2.  Python

    ``` python
    from operator import itemgetter
    myvalues = itemgetter(*mykeys)(mydict)
    ```

3.  Rust

    无

4.  swift

    无

5.  Ruby

    ``` ruby
    h = { name: 'Jack', age: 1, city: 'BJ' }
    h.values_at(:name, :city)  # => ["Jack", "BJ"]
    ```

### slice

1.  Java

    同 values<sub>at</sub>

2.  Python

    ``` python
    {k:adict[k] for k in ('key1','key2','key99') if k in adict}
    ```

3.  Rust

    无

4.  swift

    无

5.  Ruby

    ``` ruby
    h = { name: 'Jack', age: 1, city: 'BJ' }
    h.slice(:name, :city)  # => { name: 'Jack', city: 'BJ' }
    # ActiveSupport 早期提供 Hash#slice，Ruby 3.0+ 也内置
    ```

# Strings

## % / format

### Java

``` java
String.format();
System.out.printf();


String template = "Hi ${name}! Your number is ${number}";
Map<String, String> data = new HashMap<String, String>();
data.put("name", "John");
data.put("number", "1");
// commons-lang
String formattedString = StrSubstitutor.replace(template, data)
```

### Python

``` python
'hello %s %s' % ('Jack', 'David')

print('%d %s cost $%.2f' % (6, 'bananas', 1.74))

print('{0} {1} cost ${2}'.format(6, 'bananas', 1.74))
print('{quantity} {item} cost ${price}'.format(quantity=6, item='bananas', price=1.74))
```

### Rust

``` rust
format!("Hello");                 // => "Hello"
format!("Hello, {}!", "world");   // => "Hello, world!"
format!("The number is {}", 1);   // => "The number is 1"
format!("{:?}", (3, 4));          // => "(3, 4)"
format!("{value}", value=4);      // => "4"
format!("{} {}", 1, 2);           // => "1 2"
format!("{:04}", 42);             // => "0042" with leading zeros
```

### swift

``` swift
let x = "Hello"
let y = String(format: "Hello, %@", "world")
let z = String(format: "The number is %d", 1)
let a = String(format: "%@", (3, 4))
let b = String(format: "%d %d", 1, 2)
let c = String(format: "%04d", 42)
```

### Ruby

``` ruby
"hello %s %s" % ["Jack", "David"]

"%d %s cost $%.2f" % [6, 'bananas', 1.74]

format('%d %s cost $%.2f', 6, 'bananas', 1.74)

template = "Hi %{name}! Your number is %{number}"
template % { name: 'John', number: 1 }
```

## \*

### Java

``` java
Strings.repeat(); // Guava
StringUtils.repeat(); // commons-lang
```

### Python

``` python
'hello' * 3
```

### Rust

``` rust
let y = "hello";
let y = y.repeat(3);
```

### swift

``` swift
let x = "hello"
let y = String(repeating: x, count: 3)
```

### Ruby

``` ruby
"hello" * 3
```

## \<\<

### Java

``` java
StringBuilder#append()
```

### Python

无

### Ruby

``` ruby
{ a: 1, b: 2 }.inspect
%w[a b c].inspect
```

### Rust

``` rust
let mut y = String::new();
y.push_str("hello");
y.push_str(" ");
y.push_str("world");
```

### swift

``` swift
var x = "hello"
x += " world"
x.append(contentsOf: " world")
```

### Ruby

``` ruby
x = "hello"
x << " world"
x.concat(" world")
```

## =~

### Java

``` java
String#matches()
```

### Python

``` python
bool(re.match(r"hello[0-9]+", 'hello1'))
```

### Rust

``` rust
let re = Regex::new(r"(?x)
  (?P<y>\d{4}) # the year
  -
  (?P<m>\d{2}) # the month
  -
  (?P<d>\d{2}) # the day
").unwrap();

re.is_match("hello");
```

### swift

``` swift
let regex = try! Regex("[A-Za-z0-9]+")
if string.matches(of: regex).count > 0 {
    print("匹配成功")
}
```

### Ruby

``` ruby
"The quick brown fox".scan(/[a-z]+/)
"abc123DEF".scan(/[a-z]+(?:([0-9]+)|([A-Z]+))/)
```

### Ruby

``` ruby
"Hello123World456".sub(/\d+/, '***')
"Hello123World456".gsub(/\d+/) { |m| '*' * ((m.to_i % 3) + 1) }
```

### Ruby

``` ruby
/hello[0-9]+/.match?('hello1')
"hello1" =~ /hello\d+/   # 返回匹配位置或 nil
```

## bytes

### Java

``` java
String#getBytes()
```

### Python

``` python
"hello".encode()
```

### Rust

``` rust
"hello".bytes()
```

### swift

``` swift
let x = "hello"
let y = x.data(using: .utf8)
```

### Ruby

``` ruby
"hello".bytes
"hello".encode('UTF-8')
```

## capitalize

### Java

``` java
StringUtils.capitalize(); // commons-lang
```

### Python

``` python
'hello'.capitalize()
```

### Rust

无

### swift

``` swift
let x = "hello"
let y = x.capitalized
```

### Ruby

``` ruby
"hello".capitalize
```

## chars / codepoints

### Java

``` java
String#getChars();
CharSequence#chars();
CharSequence#codePoints();
```

### Python

``` python
[e for e in 'hello']
list('hello')
```

### Rust

``` rust
"hello".chars()
```

### swift

``` swift
let x = "hello"
let y = x.map { $0 }
```

### Ruby

``` ruby
"hello".chars       # => ["h", "e", "l", "l", "o"]
"hello".codepoints  # => [104, 101, 108, 108, 111]
```

## chomp!

### Java

``` java
StringUtils.chomp(); // commons-lang
```

### Python

``` python
"hello\n".rstrip('\n')
```

### Rust

`trim_*` / `strip_*`

### swift

``` swift
let x = "hello\n"
let y = x.trimmingCharacters(in: .newlines)
```

### Ruby

``` ruby
"hello\n".chomp!
"hello\n".chomp
```

## chr / ord

### Java

``` java
String#codePointAt();
new String();
String.copyValueOf();
```

### Python

chr / ord

### Rust

<https://docs.rs/asciis/0.1.3/asciis/>

### swift

``` swift
let x = "hello"
let y = x.unicodeScalars.first?.value
```

### Ruby

``` ruby
97.chr
"a".ord
```

## upcase / downcase

### Java

``` java
String#toUpperCase();
String#toLowerCase();
```

### Python

upper / lower

### Rust

to<sub>upercase</sub> / to<sub>lowercase</sub>

### swift

``` swift
let x = "hello"
let y = x.uppercased()
let z = x.lowercased()
```

### Ruby

``` ruby
"hello".upcase
"HELLO".downcase
```

## each<sub>line</sub> / lines

### Java

``` java
String#split();

new Scanner(str).useDelimiter("\n").forEachRemaining();

CharSource.wrap(str).lines();
```

### Python

``` python
'hello\nworld'.splitlines()
```

### Rust

``` rust
let y = "hello\nworld";
let y = y.lines().collect_vec();
```

### swift

``` swift
let x = "hello\nworld"
let y = x.split(separator: "\n")
```

### Ruby

``` ruby
"hello\nworld".each_line { |line| puts line }
"hello\nworld".lines
```

## `start_with?` / `end_with?`

### Java

``` java
String#startsWith();
String#endsWith();
```

### Python

starswith / endswith

### Rust

starts<sub>with</sub> / ends<sub>with</sub>

### swift

``` swift
let x = "hello"
let y = x.hasPrefix("he")
let z = x.hasSuffix("lo")
```

### Ruby

``` ruby
"hello".start_with?("he")
"hello".end_with?("lo")
```

## sub / gsub

### Java

``` java
String#replaceFirst();
String#replaceAll();
```

### Python

``` example
re.sub(pattern, repl, string, count=0, flags=0)
```

Return the string obtained by replacing the leftmost non-overlapping
occurrences of pattern in string by the replacement repl. If the pattern
isn’t found, string is returned unchanged. repl can be a string or a
function; if it is a string, any backslash escapes in it are processed.
That is, is converted to a single newline character, i̊s converted to a
carriage return, and so forth. Unknown escapes such as ȷ are left alone.
Backreferences, such as \6, are replaced with the substring matched by
group 6 in the pattern. For example:

``` example
>>> re.sub(r'def\s+([a-zA-Z_][a-zA-Z_0-9]*)\s*\(\s*\):',
...        r'static PyObject*\npy_\1(void)\n{',
...        'def myfunc():')
'static PyObject*\npy_myfunc(void)\n{'
```

If repl is a function, it is called for every non-overlapping occurrence
of pattern. The function takes a single match object argument, and
returns the replacement string. For example:

``` example
>>> def dashrepl(matchobj):
...     if matchobj.group(0) == '-': return ' '
...     else: return '-'
>>> re.sub('-{1,2}', dashrepl, 'pro----gram-files')
'pro--gram files'
>>> re.sub(r'\sAND\s', ' & ', 'Baked Beans And Spam', flags=re.IGNORECASE)
'Baked Beans & Spam'
```

The pattern may be a string or an RE object.

### Rust

``` rust
lazy_static! {
    static ref ISO8601_DATE_REGEX : Regex = Regex::new(
        r"(?P<y>\d{4})-(?P<m>\d{2})-(?P<d>\d{2})"
    ).unwrap();
}
ISO8601_DATE_REGEX.replace_all(before, "$m/$d/$y")
```

### swift

``` swift
let string = "Hello123World456"
if let regex = try? Regex("[0-9]+") {
    let newString = string.replacing(regex, with: "***")
    print(newString) // 输出: "Hello***World***"
}

//////////////////////////////////////////

let string = "Hello123World456"
if let regex = try? Regex("[0-9]+") {
    let newString = string.replacing(regex) { match in
        // match 是匹配到的数字
        let number = Int(match.0) ?? 0
        return String(repeating: "*", count: number % 3 + 1)
    }
    print(newString)
}
```

## inspect

### Java

``` java
ReflectionToStringBuilder.toString("hello", ToStringStyle.SIMPLE); // commons-lang
```

### Python

repr

### Rust

无

### swift

无

## reverse

### Java

``` java
StringBuffer#reverse();
StringBuilder#reverse();
StringUtils#reverse(); // commons-lang
```

### Python

``` python
"hello"[::-1]
```

### Rust

``` rust
let y = "hello";
let y: String = y.chars().rev().collect();
```

### swift

``` swift
var str = "Hello123World456"
var reversed_str = String(str.reversed())
```

### Ruby

``` ruby
"hello".reverse
```

## scan

### Java

``` java
java.uti.Scanner
```

### Python

``` example
>>> re.split('\W+', 'Words, words, words.')
['Words', 'words', 'words', '']
>>> re.split('(\W+)', 'Words, words, words.')
['Words', ', ', 'words', ', ', 'words', '.', '']
>>> re.split('\W+', 'Words, words, words.', 1)
['Words', 'words, words.']
>>> re.split('[a-f]+', '0a3B9', flags=re.IGNORECASE)
['0', '3', '9']
```

### Rust

``` rust
let re = Regex::new(r"[a-z]+(?:([0-9]+)|([A-Z]+))").unwrap();
let caps = re.captures("abc123").unwrap();

let text1 = caps.get(1).map_or("", |m| m.as_str());
let text2 = caps.get(2).map_or("", |m| m.as_str());
assert_eq!(text1, "123");
assert_eq!(text2, "");
```

### swift

``` swift
let string = "The quick brown fox jumps over the lazy dog"
if let regex = try? Regex("[a-z]+") {
    let words = string.matches(of: regex).map { $0.0 }
    print(words) // ["quick", "brown", "fox", "jumps", "over", "the", "lazy", "dog"]
}
```

## split

### Java

``` java
String#split();
java.util.Scanner;
```

### Python

``` python
'hello world'.split(' ')
```

### Rust

``` rust
let y = "hello world";
let y = y.split(" ").collect_vec();
```

### swift

``` swift
let x = "hello world"
let y = x.split(separator: " ")
```

### Ruby

``` ruby
"hello world".split(' ')
```

# IOs

## each / `each_line`

### Java

``` java
Files.lines(file).forEach(System.out::println);
```

### Python

``` python
filepath = 'Iliad.txt'
with open(filepath) as fp:
   for cnt, line in enumerate(fp):
       print("Line {}: {}".format(cnt, line))

# Using readlines()
file1 = open('myfile.txt', 'r')
lines = file1.readlines()
```

``` python
from pathlib import Path

Path(filename).read_text().splitlines()
```

### Rust

``` rust
let f = File::open("foo.txt")?;
let f = BufReader::new(f);

for line in f.lines() {
    println!("{}", line.unwrap());
}
```

### swift

``` swift
let path = "foo.txt"
let file = try! String(contentsOfFile: path)
let lines = file.split(separator: "\n")
```

### Ruby

``` ruby
File.foreach('foo.txt').with_index { |line, idx| puts "Line #{idx}: #{line}" }

# 或一次性读入：
File.readlines('myfile.txt')

# Ruby 3+:
require 'pathname'
Pathname('foo.txt').read.split("\n")
```

## read / readlines

### Java

``` java
Files.lines();
Files.readAllLines();
Files.readAllBytes();
FileUtils.readLines();  // commons-io
FileUtils.readFileToString();
```

### swift

``` swift
let path = "foo.txt"
let file = try! String(contentsOfFile: path, encoding: .utf8)

let lines = file.split(separator: "\n")
```

### Ruby

``` ruby
File.read('foo.txt', encoding: 'UTF-8')
File.readlines('foo.txt', chomp: true)
```

## basename / dirname

### Java

``` java
FilenameUtils.getBaseName();
FilenameUtils.getPath();
```

### Python

`os.path.basename` / `os.path.dirname`

### Rust

``` rust
use std::path::Path;
use std::ffi::OsStr;

// Note: this example does work on Windows
let path = Path::new("./foo/bar.txt");

let parent = path.parent();
assert_eq!(parent, Some(Path::new("./foo")));

let file_stem = path.file_stem();
assert_eq!(file_stem, Some(OsStr::new("bar")));

let extension = path.extension();
assert_eq!(extension, Some(OsStr::new("txt")));
```

### swift

``` swift
let path = "foo/bar.txt"
let parent = (path as NSString).deletingLastPathComponent
let file = (path as NSString).lastPathComponent
```

### Ruby

``` ruby
require 'pathname'
path = Pathname.new('foo/bar.txt')
path.dirname.to_s   # => "foo"
path.basename.to_s  # => "bar.txt"
```

## `absolute_path`

### Java

``` java
File#getAbsolutePath();
Path#toAbsolutePath();
FilenameUtils.getFullPath();
```

### Python

`os.path.abspath("mydir/myfile.txt")`

### Rust

``` rust
use std::fs;
use std::path::PathBuf;

fn main() {
    let srcdir = PathBuf::from("./src");
    println!("{:?}", fs::canonicalize(&srcdir));

    let solardir = PathBuf::from("./../solarized/.");
    println!("{:?}", fs::canonicalize(&solardir));
}
```

### swift

``` swift
let path = "foo/bar.txt"
let absolutePath = (path as NSString).expandingTildeInPath
```

### Ruby

``` ruby
require 'pathname'
Pathname.new('foo/bar.txt').realpath.to_s
```

## atime / mtime

### Java

``` java
BasicFileAttributes attrs = Files.readAttributes(file, BasicFileAttributes.class);
FileTime time = attrs.lastAccessTime();
FileTime time = attrs.lastModifiedTime();
FileTime time = attrs.lastCreationTime();

File#lastModified();
```

### Python

``` python
import os.path, time
print("last modified: %s" % time.ctime(os.path.getmtime(file)))
print("created: %s" % time.ctime(os.path.getctime(file)))
```

### Rust

<https://doc.rust-lang.org/std/fs/struct.Metadata.html>

### swift

``` swift
let path = "foo.txt"
let fileManager = FileManager.default
let attributes = try! fileManager.attributesOfItem(atPath: path)
let creationDate = attributes[.creationDate] as! Date
let modificationDate = attributes[.modificationDate] as! Date
```

### Ruby

``` ruby
st = File.stat('foo.txt')
st.atime
st.mtime
st.ctime
```

## chmod / chown

### Java

``` java
Files.setOwner();
Files.setPosixFilePermissions();

File#setReadable();
File#setWritable();
File#setExecutable();
```

### Python

`os.chmod` / `os.chown`

### Rust

<https://doc.rust-lang.org/std/fs/fn.set_permissions.html>

### swift

``` swift
let path = "foo.txt"
let fileManager = FileManager.default
// chmod
try! fileManager.setAttributes([.posixPermissions: 0o777], ofItemAtPath: path)
// chown
try! fileManager.setAttributes([.ownerAccountID: 501], ofItemAtPath: path)
```

### Ruby

``` ruby
File.chmod(0o777, 'foo.txt')
# chown 需要 uid/gid
File.chown(501, 20, 'foo.txt')
```

## directory?

### Java

``` java
Files.isDirectory();
File#isDirectory();
```

### Python

`os.path.isdir`

### Rust

``` rust
use std::fs::metadata;

fn main() {
    let md = metadata(".").unwrap();
    println!("is dir: {}", md.is_dir());
    println!("is file: {}", md.is_file());
}
```

### swift

``` swift
let path = "foo.txt"
let fileManager = FileManager.default
var isDir: ObjCBool = false
let exists = fileManager.fileExists(atPath: path, isDirectory: &isDir)
if exists {
    if isDir.boolValue {
        print("is dir")
    } else {
        print("is file")
    }
}
```

### Ruby

``` ruby
File.directory?('foo')
File.file?('foo.txt')
```

## exists?

### Java

``` java
Files.exists();
File#exist();
```

### Python

`os.path.exists`

### Rust

``` rust
use std::path::Path;

fn main() {
    println!("{}", Path::new("/etc/hosts").exists());
}
```

### swift

``` swift
let path = "foo.txt"
let fileManager = FileManager.default
let exists = fileManager.fileExists(atPath: path)
print(exists)
```

### Ruby

``` ruby
File.exist?('foo.txt')
```

## join

### Java

``` java
Paths.get(String first, String... more);
```

### Python

`os.path.join`

### Rust

``` rust
use std::path::{Path, PathBuf};

assert_eq!(Path::new("/etc").join("passwd"), PathBuf::from("/etc/passwd"));
```

### swift

``` swift
let path = "foo"
let path2 = "bar"
let joinedPath = path + "/" + path2
```

### Ruby

``` ruby
File.join('foo', 'bar') # => "foo/bar"
```

## size

### Java

``` java
Files.size();
FileUtils.sizeof(); // commons-io
```

### Python

`os.path.getsize()`

### Rust

``` rust
use std::fs;

fn main() -> std::io::Result<()> {
    let metadata = fs::metadata("foo.txt")?;

    assert_eq!(0, metadata.len());
    Ok(())
}
```

### swift

``` swift
let path = "foo.txt"
let fileManager = FileManager.default
let attributes = try! fileManager.attributesOfItem(atPath: path)
let size = attributes[.size] as! Int
```

### Ruby

``` ruby
File.size('foo.txt')
```

# Active Support

## present?

### Java

``` java
// commons-lang, spring utils
StringUtils#isNotBlank();
StringUtils#isNotEmpty();

// commons-collections, spring utils
CollectionUtils.isNotEmpty();
```

### Swift

``` swift
let x = "hello"
if !x.isEmpty {
        print("not empty")
}
```

### Ruby (Active Support)

``` ruby
require 'active_support/core_ext/object/blank'
"hello".present?   # => true
"".present?        # => false
[].present?         # => false
```

## blank?

### Java

``` java
// commons-lang, spring utils
StringUtils#isBlank();
StringUtils#isEmpty();

// commons-collections, spring utils
CollectionUtils.isEmpty();
```

### Swift

无

### Ruby (Active Support)

``` ruby
require 'active_support/core_ext/object/blank'
"".blank?        # => true
"  ".blank?      # => true
nil.blank?       # => true
[].blank?        # => true
"hello".blank?   # => false
```

## `in_groups`

### python

``` python
[l[i::n] for i in range(n)]
```

``` python
import numpy as np

[list(e) for e in np.array_split(list(range(20)), 6)]
```

### Swift

``` swift
extension Array {
    func inGroups(_ totalGroups: Int) -> [[Element]] {
        let groupSize = self.count / totalGroups
        return stride(from: 0, to: self.count, by: groupSize).map { Array(self[$0..<Swift.min($0 + groupSize, self.count)]) }
    }

    func inGroupsOf(_ groupSize: Int) -> [[Element]] {
        return stride(from: 0, to: self.count, by: groupSize).map { Array(self[$0..<Swift.min($0 + groupSize, self.count)]) }
    }
}
```

### Ruby (Active Support)

``` ruby
require 'active_support/core_ext/array/grouping'
(0...20).to_a.in_groups(6)
(0...20).to_a.in_groups(6, nil) # 填充
```

## `in_groups_of`

### Java

``` java
// guava: com.google.common.collect.Lists
public static <T> List<List<T>> partition(List<T> list, int size)
```

### python

``` python
x = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19]

[x[i:i+6] for i in range(0, len(x), 6)]
# => [[0, 1, 2, 3, 4, 5], [6, 7, 8, 9, 10, 11], [12, 13, 14, 15, 16, 17], [18, 19]]
```

### swift

``` swift
extension Array {
    func inGroups(_ totalGroups: Int) -> [[Element]] {
        let groupSize = self.count / totalGroups
        return stride(from: 0, to: self.count, by: groupSize).map { Array(self[$0..<Swift.min($0 + groupSize, self.count)]) }
    }

    func inGroupsOf(_ groupSize: Int) -> [[Element]] {
        return stride(from: 0, to: self.count, by: groupSize).map { Array(self[$0..<Swift.min($0 + groupSize, self.count)]) }
    }
}
```

### Ruby (Active Support)

``` ruby
require 'active_support/core_ext/array/grouping'
(0...20).to_a.in_groups_of(6)
(0...20).to_a.in_groups_of(6, nil) # 填充指定值
```

