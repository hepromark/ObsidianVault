OOP Important -> [[Object Oriented Programming]]
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

**Useful Objects**

Threads
```C++
#include <thread>
void work() { /* ... */ }

std::thread t(work);  // Launch new thread
t.join();             // Wait for thread to finish
t.detach();           // Thread will finish independently later
```

Mutex
```C++
void safe_function() {
    std::lock_guard<std::mutex> lock(mtx);  // auto-releases on scope exit
    // critical section
}
```

Locks: lock_guard 
```C++
std::mutex mtx;

void locking_function() {
	std::lock_guard<std::mutex> lock(mtx);  // Locks on declaration
}  // Auto unlocks when lock goes out of scope
```

unique_lock
- Supports manual lock & unlock
```C++
std::mutex mtx;

void locking_function() {
	std::unique_lock<std::mutex> lock(mtx);  // Locks on declaration
	// Crit sec
	lock.unlock();
	// Outside crit sec
	lock.lock(); // Relock if needed
} // Also auto unlocks when out of scope
```

condition_variable
- A clean way to implement "unblock check condition, if can't proceed relock else lock & proceed"
- Tied to a condition checker and a specific mutex
- Releases the lock while waiting
```C++
std::mutex mtx;
std::condition_variable cv;
bool ready = false;

void wait_thread() {
    std::unique_lock<std::mutex> lock(mtx); // Acquires the lock
    cv.wait(lock, [] { return ready; });  // Wait until ready == true, if not true unblock
    std::cout << "Thread resumed\n";
}

void signal_thread() {
    {
        std::lock_guard<std::mutex> lock(mtx);
        ready = true;
    }
    cv.notify_one();  // Wake up one waiting thread
}
```

semaphore
```
std::counting_semaphore<10> sem(3);  // 10 is max, 3 is starting
std::binary_semaphore bin_sem(1);    // init to 1

// Operations
.release()  <=> signal()
.acquire()  <=> acquire()
```

**IN C**
```
sem_t sem;
sem_init(&sem, 0, intitial_value);

// Operations
sem_wait(&sem);
sem_post(&sem);

sem_destroy(&sem);
```
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

 `std::bitset`
- C++ class for more safe, readable bit operations
- Overhead
- Fixed size at compile time
- Can't do bitwise operations directly w/ integers (need conversion first)

General strategies:
- Bit masking: 1 << n (a single 1 at bit position n)
- XOR is associative -> XOR 2 identical numbers will give 0, while 0 XOR n -> n
- `uint64_t biggest_32_bit_value = 1ULL << 32` to get $2^{32}$ 

# STL Objects

### unordered_map
`std::unordered_map<KeyType, ValueType> map;

**Insertion**
```C++
map[key] = value;
map.insert({key, value});  // Inserts <=> Key doesn't exist
map.emplace(key, value);  // More efficient insert
```

**Lookup**
```C++
value = map[key];  // Returns value or inserts defauult key if it doesn't exist
map.at(key);       // Returns value or throws std::out_of_range

map.find(key);     // Return iterator or map.end() if not found
map.count(key);    // 1 if key exists, 0 otherwise
```

**Erase**
```C++
map.erase(key);
map.clear();
```

**Misc**
```C++
map.size();
map.clear();


// Custom Hash
struct PairHash {
	std::size_t operator()(const std::pair<int, int>& p) const {
		return std::hash<int>()(p.first) ^ (std::hash<int>()(p.second) << 1);
	}
}

std::unordered_map<std::pair<int, int>, int, PairHash> map;
	
```
### map

### unordered_set

### set

### vector

