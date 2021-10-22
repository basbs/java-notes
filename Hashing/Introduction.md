# Hashing
Hashing is one of the way to store key value pairs in a table in an un-ordered fashion. 
Hash function takes the key and reduces to array index from key.

# Challenges
* Compute the hash function.
* Equality test: method for checking whether two keys are equal. 
* Collision resolution: Algorithm and data structure to handle two keys that hash to the same array index. 
* Classis space time tradeoff: if we don't have space limit, then we can keep storing the data forever and same goes for time as well. if we have enough time storing all the data in one location and iterate them linearly until we get to the actual value. 


# Hash Function
* While generating hash key, each table index equally likely for each key. 
* Every type of key needs a unique hash technique to avoid more collision. 
* All java classes inherit a method hashCode(), which returns a 32-bit integer value. 
* If x.equals(y), then (x.hashCode() == y.hashCode()). similarly !x.equals(y), then (x.hashCode() != y.hashCode())
* Since String is immutable, java internally caches the once computed hashCode and returns the same code when hashCode method called more than once. 

Example: If we do the hashing for STD codes in phone number list, we end up storing all the state phone numbers in one location which ends up having a linked list at one index. 

# Hash Code Design
* Combine each significant field using the 31x + y rule. where x is a prime number and y is the actual field which involves in hash strategy. 
* If field is a primitive type, use wrapper type hashCode().
* If field is null, return 0.
* If field is a reference type, use hashCode(). 
* If field is an array, apply to each entry. [ Arrays.deepHashCode()]
* If the requirement is to use the entire object for hashKey (take care of the performance)
* Hash code will be an int between -2^31 and 2^31
* If we have a array of size M, we need a **hash function** which should give an integer in return between 0 and M - 1 (for use as array index) The M will be typically prime or power of 2. 
* Why M always has to be power of 2 or prime?
* `private int hash(Key key) { return key.hashCode() % M;`
* The above code returns a negative value, hence taking an absolute value avoid the negative index.
* `private int hash(Key key) { return Math.abs(key.hashCode()) % M; }` even after taking absolute value, -2^31 still be negative. 
* `private int hash(Key key) { return (key.hashCode() & 0x7fffffff) % M; }` 
* The above piece of code will generate the hashCode converts it into positive number, then takes the modulo value. 
* In theory, uniform hashing assumes that, each key is equally likely to hash to an integer between 0 and M - 1. 
* Bins and balls - Throw balls uniformly at random into M bins. 
* Birthday problem. 

## Collision Avoid:
Collision: two distinct keys hashing to the same index. 
How to delete?
How to resize?

### Technique-1: Separate Chaining
**Use an array of M < N linked lists.**
1. Hash: map key to integer i between 0 and M-1
2. Insert: put at front of ith chain(i.e. insert at the linked list)
3. Search: need to search only ith chain. 
#### Advantage
* Easier to implement delete.
* Performance degrades gracefully.
* Clustering less sensitive to poorly-designed hash function.

### Linear Probing or Open Addressing.
* Use an array instead of array of linked list. 
* When a new key collides, find next empty slot, and put it there. 
#### Advantage
* Less wasted space.
* Better cache performance. 

__Steps__
1. Array size M must be greater than number of key-value pairs N. 
1. **Hash** Map key to integer i between 0 and M-1.
2. **Insert** Put at table index i if free; if not try i+1, i+2, etc. 
3. **Search** Search table index i; if occupied but no match, try i+1, i+2, etc. 
3. This works efficiently as long as we have bigger array size. 
4. If we keep going at i+1 pace and encounter the last index, then the pointer has to reset to the beginning and try the i+1 approach again. 
5. If we get any key which is not present in table at all, then the moment we hit the empty cell we assume that the value doesn't exist. 
6. This way of filling the values in hash table is similar to parking a car in one-way. When someone wants to park a car, he comes parks at say X location. If other car comes at the same time, he will have to look for X+1 location to park. as the car number grows the last person comes has to take very long route to park his car. 
7. **Model** Cars arrive at one-way street with M parking spaces. Each desires a random space i; if space i is taken, try i+1, i+2, etc. 
8. If parking is **Half-ful** with M/2 cars, mean displacement is ~ 3/2
9. If parking is **Full** with M cars, mean displacement is ~ sqrt Pi * M/8.
10. M too large => too many empty array entries. 
11. M too small => search time blows up.
12. Typical choice: alpha = N/M ~ 1/2. (number of probes for search hit is about 3/2 and number of probes for search miss is about 5/2)

### Practical Observation
* In Java 1.1 to generate a hash code for long strings costs a lot of CPU. i.e. they only examined 8-9 evenly spaced characters. The advantage with this is saves time but the downside was if a bunch of string (Ex: URL) has same characters in those location, it ended up giving the same hash values for all of them. 

#### One-way hash functions
* hard to find a key that will hash to a desired value (or two keys that hash to same value)

### Two-probe Hashing

### Hash tables vs Balance search trees.
#### Hash Tables
* Simpler to code.
* No effective alternative for unordered keys.
* Faster for simple keys (a few arithmetic ops versus logN compares)
* Better system support in java for string (Ex: cached hash code)
* In Java java.util.HashMap, java.util.IdentityHashMap

#### Balanced search trees.
* Stronger performance guarantee.
* Support for ordered ST operations.
* Easier to implement compareTo() correctly than equals() and hashCode().
* In java: java.util.TreeMap, java.util.TreeSet.




