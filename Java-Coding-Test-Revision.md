# Java Revision Sheet: Collections Framework + String Manipulation
For your BT Group OA and upcoming technical interviews, these two topics are worth revising together. You want to be able to **recall the syntax quickly, choose the right data structure, and avoid edge-case bugs** without wasting time during coding.

This sheet is designed for quick revision, especially for array/string coding questions, Java MCQs, and interview problems.

---

# Part 1 — Java Collections Framework

## 1. The hierarchy you should remember
Java Collections provide data structures for storing and manipulating groups of objects.

The most important interfaces are:

InterfacePurposeCommon implementations`List`Ordered collection; duplicates allowed`ArrayList`, `LinkedList``Set`Unique elements`HashSet`, `LinkedHashSet`, `TreeSet``Queue`Elements waiting to be processed`LinkedList`, `PriorityQueue``Deque`Insertion/removal at both ends`ArrayDeque`, `LinkedList``Map`Key-value pairs; unique keys`HashMap`, `LinkedHashMap`, `TreeMap`

**Important distinction:** `Map` is part of the Java Collections Framework, but it does not extend the `Collection` interface.

### Quick selection guide

- Need indexed access and duplicates? → `ArrayList`
- Need unique elements? → `HashSet`
- Need unique elements in sorted order? → `TreeSet`
- Need key-value frequency counting? → `HashMap`
- Need keys sorted? → `TreeMap`
- Need a min-heap? → `PriorityQueue`
- Need stack or queue operations? → Usually `ArrayDeque`

---

## 2. ArrayList — your everyday list

### Declaration and initialization

```
import java.util.*;

ArrayList<Integer> list = new ArrayList<>();

List<Integer> nums = new ArrayList<>(
    Arrays.asList(10, 20, 30)
);

List<String> names = new ArrayList<>(
    List.of("Aman", "Riya", "Sam")
);
```

`List.of()` creates an unmodifiable list. Use a mutable implementation such as `ArrayList` when you need to add, remove, or replace elements.

### Essential methods

```
list.add(10);             // Add at the end
list.add(0, 5);           // Insert at index 0
list.get(0);              // Access element
list.set(0, 100);         // Replace element
list.remove(0);           // Remove by index
list.remove(Integer.valueOf(10)); // Remove by value
list.size();              // Number of elements
list.isEmpty();           // Check if empty
list.contains(10);        // Check if value exists
list.indexOf(10);         // First matching index, or -1
list.clear();             // Remove all elements
```

### The classic Java trap: `remove()`

```
ArrayList<Integer> nums = new ArrayList<>(
    Arrays.asList(10, 20, 30)
);

nums.remove(1);                    // Removes index 1: value 20
nums.remove(Integer.valueOf(10)); // Removes the value 10
```

Why? `remove(int index)` and `remove(Object value)` are different overloads.

### Traversal

```
for (int i = 0; i < list.size(); i++) {
    System.out.println(list.get(i));
}

for (int x : list) {
    System.out.println(x);
}
```

**Complexity:**

OperationTypical complexity`get(index)`O(1)`set(index, value)`O(1)`add(value)` at endAmortized O(1)Insert/remove at beginning or middleO(n)`contains(value)`O(n)

Use `ArrayList` as your default list unless the problem suggests something else.

---

## 3. HashSet — uniqueness and fast membership checks
A `HashSet` stores unique elements and does not guarantee iteration order.

```
Set<Integer> set = new HashSet<>();

set.add(10);
set.add(20);
set.add(10); // Duplicate ignored

System.out.println(set);          // Order not guaranteed
System.out.println(set.contains(20)); // true

set.remove(10);
System.out.println(set.size());
```

Average-case `add()`, `remove()`, and `contains()` are O(1), although their worst-case complexity can be worse.

### Common interview use: detecting duplicates

```
int[] arr = {1, 2, 3, 2};

Set<Integer> seen = new HashSet<>();
boolean duplicate = false;

for (int x : arr) {
    if (!seen.add(x)) {
        duplicate = true;
        break;
    }
}
```

`Set.add(x)` returns `false` if the element already exists.

### Which Set should you use?
ImplementationOrderingTypical operation cost`HashSet`No guaranteed orderAverage O(1)`LinkedHashSet`Insertion orderAverage O(1)`TreeSet`Sorted orderO(log n)

**Trap:** Do not assume a `HashSet` prints elements in insertion or sorted order.

---

## 4. HashMap — one of the most important structures for DSA
A `HashMap` stores key-value pairs. Each key is unique, but multiple keys can have the same value.

```
Map<String, Integer> map = new HashMap<>();

map.put("Apple", 3);
map.put("Banana", 2);
map.put("Apple", 5); // Overwrites previous value

System.out.println(map.get("Apple")); // 5
System.out.println(map.get("Mango")); // null
```

### Essential methods

```
map.put(key, value);
map.get(key);
map.getOrDefault(key, defaultValue);
map.containsKey(key);
map.containsValue(value);
map.putIfAbsent(key, value);
map.remove(key);
map.size();
map.isEmpty();
map.clear();
```

### The most useful method: `getOrDefault()`
Suppose you need to count frequencies in an array.

```
int[] arr = {1, 2, 2, 3, 1, 2};

Map<Integer, Integer> freq = new HashMap<>();

for (int x : arr) {
    freq.put(x, freq.getOrDefault(x, 0) + 1);
}

System.out.println(freq);
```

Conceptually:

```
1 -> 2
2 -> 3
3 -> 1
```

This pattern is essential for:

- Finding duplicates.
- Finding the first non-repeating character.
- Checking anagrams.
- Counting element frequencies.
- Two Sum.
- Subarray Sum Equals K.

### Iterating over a HashMap

```
for (int key : map.keySet()) {
    System.out.println(key);
}
```

```
for (int value : map.values()) {
    System.out.println(value);
}
```

```
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    System.out.println(
        entry.getKey() + " " + entry.getValue()
    );
}
```

The `entrySet()` approach is especially useful when you need both keys and values.

### Advanced frequency-counting pattern

```
map.putIfAbsent(key, 0);
map.put(key, map.get(key) + 1);
```

Or simply:

```
map.merge(key, 1, Integer::sum);
```

For an OA, `getOrDefault()` is usually the easiest pattern to remember.

### HashMap vs LinkedHashMap vs TreeMap
StructureKey orderingTypical `get()``HashMap`No guaranteed orderAverage O(1)`LinkedHashMap`Insertion order by defaultAverage O(1)`TreeMap`Sorted by keyO(log n)

**Interview trap:** `HashMap` permits one `null` key and multiple `null` values. `TreeMap` normally requires mutually comparable keys and does not allow a `null` key with its natural ordering.

---

## 5. Stack, Queue and Deque

### Stack
For modern Java code, prefer `ArrayDeque` when you need ordinary stack operations.

```
Deque<Integer> stack = new ArrayDeque<>();

stack.push(10);
stack.push(20);
stack.push(30);

System.out.println(stack.peek()); // 30
System.out.println(stack.pop());  // 30
System.out.println(stack.peek()); // 20
```

MethodPurpose`push(x)`Add to top`pop()`Remove and return top`peek()`Read top without removing

A stack follows **LIFO**: Last In, First Out.

Typical uses:

- Valid parentheses.
- Next Greater Element.
- Reversing data.
- Monotonic stack problems.
- DFS.
**Edge case:** `ArrayDeque.pop()` and `ArrayDeque.getFirst()` throw an exception when the deque is empty. Check `isEmpty()` when necessary.

### Queue

```
Queue<Integer> queue = new ArrayDeque<>();

queue.offer(10);
queue.offer(20);
queue.offer(30);

System.out.println(queue.peek()); // 10
System.out.println(queue.poll()); // 10
System.out.println(queue.peek()); // 20
```

A queue follows **FIFO**: First In, First Out.

MethodBehaviour when operation fails`offer(x)`Returns `false` if insertion fails`poll()`Returns `null` if empty`peek()`Returns `null` if empty`remove()`Throws exception if empty`element()`Throws exception if empty

### Deque
A `Deque` supports insertion and removal at both ends.

```
Deque<Integer> deque = new ArrayDeque<>();

deque.offerFirst(10);
deque.offerLast(20);

System.out.println(deque.peekFirst()); // 10
System.out.println(deque.peekLast());  // 20

deque.pollFirst();
deque.pollLast();
```

Useful for:

- Sliding window maximum.
- Implementing a stack or queue.
- Palindrome checks.
- Monotonic deque problems.

---

## 6. PriorityQueue — heaps in Java
By default, Java's `PriorityQueue` is a **min-heap**.

```
PriorityQueue<Integer> pq = new PriorityQueue<>();

pq.offer(30);
pq.offer(10);
pq.offer(20);

System.out.println(pq.peek()); // 10
System.out.println(pq.poll()); // 10
System.out.println(pq.poll()); // 20
```

### Max-heap

```
PriorityQueue<Integer> maxHeap =
    new PriorityQueue<>(Collections.reverseOrder());

maxHeap.offer(10);
maxHeap.offer(30);
maxHeap.offer(20);

System.out.println(maxHeap.peek()); // 30
```

### Complexity
OperationComplexity`offer()`O(log n)`poll()`O(log n)`peek()`O(1)Construct heap from a collectionO(n) with the collection constructor

### Common uses

- Kth-largest or Kth-smallest element.
- Top K frequent elements.
- Merge K sorted lists.
- Scheduling problems.
- Dijkstra's algorithm.
**Trap:** Iterating through a `PriorityQueue` does not guarantee sorted order. Repeatedly call `poll()` to retrieve elements in priority order.

---

## 7. Sorting collections and arrays

### Sort a list

```
List<Integer> nums = new ArrayList<>(
    Arrays.asList(5, 1, 4, 2, 3)
);

Collections.sort(nums);

System.out.println(nums); // [1, 2, 3, 4, 5]
```

Or:

```
nums.sort(null);
```

### Descending order

```
nums.sort(Collections.reverseOrder());
```

This works for lists of compatible `Comparable` elements, such as `Integer` and `String`.

### Sort an array

```
int[] arr = {5, 2, 8, 1};

Arrays.sort(arr);
```

For an `Integer[]`:

```
Integer[] arr = {5, 2, 8, 1};

Arrays.sort(arr, Collections.reverseOrder());
```

**Trap:** `Collections.reverseOrder()` cannot be used as the comparator for a primitive `int[]`. Use `Integer[]`, or sort the primitive array ascending and reverse it yourself.

### Custom comparator
Suppose you have a list of strings and want to sort by length:

```
List<String> words = new ArrayList<>(
    Arrays.asList("banana", "cat", "apple", "hi")
);

words.sort((a, b) -> Integer.compare(a.length(), b.length()));
```

For sorting integers in descending order, prefer:

```
nums.sort((a, b) -> Integer.compare(b, a));
```

Avoid `b - a` as a comparator because integer overflow can produce incorrect ordering.

### Binary search

```
int[] arr = {1, 3, 5, 7, 9};

int index = Arrays.binarySearch(arr, 7);
```

The array must be sorted first for this use.

If the element isn't found, `Arrays.binarySearch()` returns a negative value encoding an insertion point. Don't assume it returns exactly `-1`.

For coding problems requiring a specific lower bound or upper bound, implementing binary search manually is often clearer.

---

## 8. Other collection utilities worth knowing

```
Collections.reverse(list);
Collections.sort(list);
Collections.max(list);
Collections.min(list);
Collections.frequency(list, 5);
Collections.swap(list, 0, 1);

Arrays.sort(arr);
Arrays.fill(arr, -1);
Arrays.equals(arr1, arr2);
Arrays.toString(arr);
```

### Convert between arrays and lists

```
int[] arr = {1, 2, 3};
```

For a primitive array, this does **not** produce a `List<Integer>`:

```
// Arrays.asList(arr) treats the int[] as one object.
```

Use a loop or streams to convert a primitive `int[]` to a `List<Integer>`.

For an object array:

```
Integer[] arr = {1, 2, 3};

List<Integer> list = new ArrayList<>(Arrays.asList(arr));
```

The `new ArrayList<>(...)` creates a mutable list.

---

# Part 2 — Java String Manipulation

## 9. The most important rule: Strings are immutable
A Java `String` cannot be modified after creation. Operations that appear to modify a string return a new string.

```
String s = "hello";

s.toUpperCase();

System.out.println(s); // hello
```

To retain the result:

```
s = s.toUpperCase();

System.out.println(s); // HELLO
```

This matters in interviews because repeated string concatenation inside a loop can be inefficient.

---

## 10. Essential String methods
Assume:

```
String s = "Hello World";
```

MethodExampleResult`length()``s.length()``11``charAt(i)``s.charAt(0)``'H'``substring(start)``s.substring(6)``"World"``substring(start, end)``s.substring(0, 5)``"Hello"``indexOf()``s.indexOf('o')``4``lastIndexOf()``s.lastIndexOf('o')``7``contains()``s.contains("World")``true``equals()``s.equals("Hello World")``true``equalsIgnoreCase()``s.equalsIgnoreCase("hello world")``true``toCharArray()``s.toCharArray()`Character array`toLowerCase()``s.toLowerCase()`Lowercase string`toUpperCase()``s.toUpperCase()`Uppercase string`trim()``"  hi  ".trim()``"hi"``isEmpty()``"".isEmpty()``true``startsWith()``s.startsWith("Hello")``true``endsWith()``s.endsWith("World")``true``replace()``s.replace("World", "Java")``"Hello Java"`

### Critical edge cases
**1. `substring()` excludes the ending index.**

```
String s = "abcdef";

System.out.println(s.substring(1, 4)); // bcd
```

The range is `[start, end)`, meaning the start is included and the end is excluded.

**2. `charAt()` uses zero-based indexing.**

```
s.charAt(0); // 'a'
s.charAt(5); // 'f'
```

Calling `charAt(s.length())` throws `StringIndexOutOfBoundsException`.

**3. `isEmpty()` and `isBlank()` differ.**

```
"".isEmpty();       // true
"   ".isEmpty();    // false
"   ".isBlank();    // true
```

`isBlank()` is available in Java 11 and later.

**4. `trim()` and `strip()` differ.**

`trim()` removes leading and trailing characters whose values are at most U+0020. `strip()` uses Unicode whitespace rules and is available in Java 11 and later.

---

## 11. String comparison: `==` vs `equals()`
This is a very common Java interview question.

```
String a = new String("hello");
String b = new String("hello");

System.out.println(a == b);      // false
System.out.println(a.equals(b)); // true
```

- `==` compares object references for objects.
- `equals()` compares string contents.
String literals may be interned, which can make `==` return `true` in some cases:

```
String x = "hello";
String y = "hello";

System.out.println(x == y); // true in this example
```

**Interview rule: use `equals()` when comparing string content.**

---

## 12. Convert between String, char array and integer

### String → char array

```
String s = "hello";

char[] chars = s.toCharArray();
```

Modify characters using the array:

```
chars[0] = 'H';

String result = new String(chars);

System.out.println(result); // Hello
```

### Character array → String

```
char[] chars = {'J', 'a', 'v', 'a'};

String s = new String(chars);
```

### String → integer

```
int num = Integer.parseInt("123");
long bigNum = Long.parseLong("12345678900");
double decimal = Double.parseDouble("12.5");
```

These methods throw `NumberFormatException` if the string is not a valid number.

### Integer → String

```
int n = 123;

String s1 = String.valueOf(n);
String s2 = Integer.toString(n);
```

### Character conversion

```
char ch = 'a';

Character.isLetter(ch);     // true
Character.isDigit(ch);      // false
Character.isLetterOrDigit(ch);
Character.isWhitespace(' ');
Character.toUpperCase(ch);  // 'A'
Character.toLowerCase('A'); // 'a'
```

These methods are useful when the input may contain spaces, punctuation, digits, or mixed case.

---

## 13. StringBuilder — essential for efficient string problems
Use `StringBuilder` when constructing or modifying a string repeatedly.

```
StringBuilder sb = new StringBuilder();

sb.append("Hello");
sb.append(" ");
sb.append("World");

System.out.println(sb.toString()); // Hello World
```

### Essential methods

```
StringBuilder sb = new StringBuilder("hello");

sb.append(" world");
sb.charAt(0);
sb.setCharAt(0, 'H');
sb.insert(5, "!");
sb.delete(5, 6);
sb.reverse();
sb.length();
sb.toString();
```

### Reverse a string

```
String s = "hello";

String reversed = new StringBuilder(s)
        .reverse()
        .toString();

System.out.println(reversed); // olleh
```

### Reverse a character array manually

```
char[] arr = s.toCharArray();

int left = 0;
int right = arr.length - 1;

while (left < right) {
    char temp = arr[left];
    arr[left] = arr[right];
    arr[right] = temp;

    left++;
    right--;
}

String reversed = new String(arr);
```

This technique is useful when the question asks you to reverse characters in place.

### String vs StringBuilder
FeatureStringStringBuilderMutable?NoYesRepeated modificationsCan create many objectsGenerally more efficientThread-safe?ImmutableNot synchronizedCommon useFixed text, comparisonsConstructing/modifying text

**Performance tip:** Avoid this pattern for large inputs:

```
String result = "";

for (int i = 0; i < 10000; i++) {
    result += i;
}
```

Prefer:

```
StringBuilder result = new StringBuilder();

for (int i = 0; i < 10000; i++) {
    result.append(i);
}
```

---

## 14. Split strings and manipulate words

### Split by spaces

```
String s = "Java is very useful";

String[] words = s.split(" ");

for (String word : words) {
    System.out.println(word);
}
```

### Split on arbitrary whitespace

```
String s = "Java   is\tvery useful";

String[] words = s.trim().split("\\s+");
```

For an empty or whitespace-only input, handle the case separately if your algorithm depends on the number of words.

### Split on a dot
The `split()` method accepts a regular expression. A dot has special meaning in regex, so escape it:

```
String s = "file.txt";

String[] parts = s.split("\\.");

System.out.println(parts[0]); // file
System.out.println(parts[1]); // txt
```

To preserve trailing empty fields, use a negative limit:

```
String s = "a,b,";

String[] parts = s.split(",", -1);
```

This retains the empty element after the final comma.

---

## 15. Character frequency counting
This is one of the highest-value patterns for string questions.

### When the input contains only lowercase English letters

```
String s = "banana";

int[] freq = new int[26];

for (int i = 0; i < s.length(); i++) {
    char ch = s.charAt(i);
    freq[ch - 'a']++;
}
```

The frequency table becomes:

```
a -> 3
b -> 1
n -> 2
```

To retrieve a character's count:

```
int count = freq['a' - 'a']; // 3
```

**Important:** This approach assumes every character is between `'a'` and `'z'`. It will not work safely for uppercase letters, spaces, or arbitrary Unicode characters.

### When the input may contain arbitrary characters

```
Map<Character, Integer> freq = new HashMap<>();

for (char ch : s.toCharArray()) {
    freq.put(ch, freq.getOrDefault(ch, 0) + 1);
}
```

### Find the first non-repeating character

```
public static int firstUniqueChar(String s) {
    int[] freq = new int[26];

    for (char ch : s.toCharArray()) {
        freq[ch - 'a']++;
    }

    for (int i = 0; i < s.length(); i++) {
        if (freq[s.charAt(i) - 'a'] == 1) {
            return i;
        }
    }

    return -1;
}
```

Why two passes?

1. The first pass counts every character.
2. The second pass finds the first character whose total frequency is one.
The second pass is important because a character that appears unique early in the string may occur again later.

Complexity: O(n) time and O(1) auxiliary space for lowercase English letters.

---

## 16. Common string coding patterns

### A. Check whether a string is a palindrome

```
public static boolean isPalindrome(String s) {
    int left = 0;
    int right = s.length() - 1;

    while (left < right) {
        if (s.charAt(left) != s.charAt(right)) {
            return false;
        }

        left++;
        right--;
    }

    return true;
}
```

This version compares characters exactly. It does **not** ignore spaces, punctuation, or case.

For example, `"Madam"` returns `false` because `'M' != 'm'`.

### B. Check whether two strings are anagrams
For lowercase English letters:

```
public static boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) {
        return false;
    }

    int[] freq = new int[26];

    for (int i = 0; i < s.length(); i++) {
        freq[s.charAt(i) - 'a']++;
        freq[t.charAt(i) - 'a']--;
    }

    for (int count : freq) {
        if (count != 0) {
            return false;
        }
    }

    return true;
}
```

Complexity: O(n) time and O(1) auxiliary space.

**Edge case:** Check the lengths first. Also remember that this implementation assumes lowercase English letters.

### C. Remove duplicate characters while preserving first occurrence

```
public static String removeDuplicates(String s) {
    boolean[] seen = new boolean[26];
    StringBuilder result = new StringBuilder();

    for (char ch : s.toCharArray()) {
        int index = ch - 'a';

        if (!seen[index]) {
            seen[index] = true;
            result.append(ch);
        }
    }

    return result.toString();
}
```

Example:

```
Input:  "programming"
Output: "progamin"
```

This version also assumes lowercase English letters.

### D. Count vowels

```
public static int countVowels(String s) {
    int count = 0;

    for (char ch : s.toLowerCase().toCharArray()) {
        if (ch == 'a' || ch == 'e' || ch == 'i'
                || ch == 'o' || ch == 'u') {
            count++;
        }
    }

    return count;
}
```

Example:

```
Input:  "Anirudh"
Output: 3
```

---

# Part 3 — High-Value Patterns for Your OA

## 17. Choose the right data structure
This table is worth memorizing.

Problem asks you to…UseFind duplicates`HashSet`Count frequencies`HashMap` or frequency arrayFind first non-repeating characterFrequency array + second passCheck anagramsFrequency array or `HashMap`Retrieve the largest or smallest element repeatedly`PriorityQueue`Find the Kth-largest elementMin-heap of size KMatch opening and closing bracketsStackProcess elements in arrival orderQueueMaintain sorted unique values`TreeSet`Maintain sorted key-value pairs`TreeMap`Check whether a prefix existsTrie, for suitable problemsBuild a string repeatedly`StringBuilder`Reverse a string`StringBuilder` or two pointersFind a substring with constraintsSliding windowFind subarrays with a target sumPrefix sum + `HashMap`, when appropriate

### A particularly important distinction
**Finding whether a value exists** and **counting how many times it appears** are different tasks.

```
Set<Integer> seen = new HashSet<>();
```

Use this when you only care about membership or uniqueness.

```
Map<Integer, Integer> freq = new HashMap<>();
```

Use this when you need counts.

For example, `[1, 1, 1, 2]` has two distinct values but four total elements.

---

## 18. Common Java traps in coding assessments
These are the mistakes I would particularly watch for in your solutions.

### Trap 1: Comparing strings with `==`
Incorrect:

```
if (s1 == s2) {
    // ...
}
```

Correct for comparing contents:

```
if (s1.equals(s2)) {
    // ...
}
```

### Trap 2: Confusing `length` and `length()`

```
arr.length;       // Array
s.length();       // String
list.size();      // Collection
```

### Trap 3: Forgetting that strings are immutable

```
s.replace('a', 'b');
```

This does not change `s`. Assign the returned value if you need the result:

```
s = s.replace('a', 'b');
```

### Trap 4: Assuming a HashMap is sorted

```
Map<Integer, Integer> map = new HashMap<>();
```

The iteration order is not guaranteed. Use `TreeMap` if sorted key order is required.

### Trap 5: Integer overflow
Suppose you need the sum of two integers:

```
int sum = a + b;
```

If the result exceeds the range of `int`, it overflows.

When constraints require it, use:

```
long sum = (long) a + b;
```

Casting before addition matters.

### Trap 6: Modifying a collection during enhanced-for iteration
This can cause `ConcurrentModificationException`:

```
for (Integer x : list) {
    if (x < 0) {
        list.remove(x);
    }
}
```

For a list, use an iterator when removing during traversal:

```
Iterator<Integer> it = list.iterator();

while (it.hasNext()) {
    if (it.next() < 0) {
        it.remove();
    }
}
```

### Trap 7: Using `split()` without understanding regex

```
s.split(".");
```

A dot means "any character" in regular expressions. To split on an actual dot, use:

```
s.split("\\.");
```

### Trap 8: Treating a PriorityQueue as a sorted list

```
PriorityQueue<Integer> pq = new PriorityQueue<>();
```

`pq.peek()` gives the minimum, but iterating over `pq` does not guarantee sorted order.

### Trap 9: Returning the wrong index
When a problem asks for the index of the first unique character, return `i`, not the character itself.

```
return i;               // Index
return s.charAt(i);     // Character
```

The method's return type should match the question.

---

# Part 4 — Last-Minute Syntax Cheat Sheet
Use this as a quick reference before the assessment.

### Collections

```
// List
List<Integer> list = new ArrayList<>();
list.add(10);
list.get(0);
list.set(0, 20);
list.remove(Integer.valueOf(20));

// Set
Set<Integer> set = new HashSet<>();
set.add(10);
set.contains(10);
set.remove(10);

// Map
Map<Integer, Integer> map = new HashMap<>();
map.put(1, 100);
map.getOrDefault(1, 0);
map.containsKey(1);

// Stack
Deque<Integer> stack = new ArrayDeque<>();
stack.push(10);
stack.peek();
stack.pop();

// Queue
Queue<Integer> queue = new ArrayDeque<>();
queue.offer(10);
queue.peek();
queue.poll();

// Min-heap
PriorityQueue<Integer> minHeap = new PriorityQueue<>();

// Max-heap
PriorityQueue<Integer> maxHeap =
    new PriorityQueue<>(Collections.reverseOrder());

// Sorting
Collections.sort(list);
list.sort(Collections.reverseOrder());
Arrays.sort(arr);

// Reverse a list
Collections.reverse(list);
```

### Strings

```
// Length
s.length();

// Character at index
s.charAt(i);

// Substring [start, end)
s.substring(start, end);

// Content comparison
s.equals(t);
s.equalsIgnoreCase(t);

// Character array
char[] chars = s.toCharArray();

// StringBuilder
StringBuilder sb = new StringBuilder();
sb.append("hello");
sb.reverse();
String result = sb.toString();
```
