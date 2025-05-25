# Object Orientated Programming

### Encapsulation

### Abstraction
### Inheritance
### Polymorphism

# Smart Pointers

Purpose:
- Prevent memory leaks and dangling pointers (safer to use)
- Smart pointers release memory automatically when they go out of scope

Types:
1. `std::unique_ptr`
	- Exclusive ownership, only 1 unique_ptr can own a resource
	- Non-copyable
	- Moveable

```C++
std::unique_ptr<MyClass> ptr = std::make_unique<MyClass>();

// We can't copy ptr like this:
std::unique_ptr<MyClass> ptr_2 = ptr;

// But we can move ownership
std::unique_ptr<MyClass> ptr_2 = std::move(ptr);
```

2. `std::shared_ptr`
	- Shared ownership, multiple shared_ptr can own a resource
	- Uses *reference counting*

```C++
std::shared_ptr<MyClass> ptr = std::make_shared<MyClass>();

// To delete
ptr.reset();
```

3. `std::weak_ptr`
	- Non-owning reference to a `shared_ptr` managed object
	- Prevents cyclic references of `shared_ptr`
	- To access resource, needs to be converted to `shared_ptr`;

```C++
std::shared_ptr<MyClass> shared = std::make_shared<MyClass>();

std::weak_ptr<MyClass> weak = shared;

// Convert weak -> shared to access obj
if (std::weak_ptr<MyClass> temp = weak.lock()) {
	temp->doSomething();
} else {
	std::cout << "Object no longer accessible because it's been deleted";
}
```

# Multi-threading


# Bitwise Operation

![[Pasted image 20250524144552.png]]
**XOR:**
- Communicative & Associative
- `a ^ b == b ^ a`
- `(a ^ b) ^ c == a ^ (b ^ c)`

**AND**:
- Communicative & Associative

OR: 
- Communicative & Associative

NAND:

- not communicative, not associative
- Is a universal gate
### `std::bitset`
- C++ class for more safe, readable bit operations
- Overhead
- Fixed size at compile time
- Can't do bitwise operations directly w/ integers (need conversion first)

**Leetcode Practice**
- https://neetcode.io/problems/single-number

General strategies:
- Bit masking: 1 << n (a single 1 at bit position n)
- XOR is associative -> XOR 2 identical numbers will give 0, while 0 XOR n -> n
- `uint64_t biggest_32_bit_value = 1ULL << 32` to get $2^{32}$ 
