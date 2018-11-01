---
title: Code Bits - Working with COM interfaces
---
Every once in a while I'll take a closer look at interesting code I wrote. These won't be earth shocking but hopefully these posts contain some cool tidbits of information. In this one I'll be looking at COM interfaces, more specifically at managing  pointers to them.

When working with COM interfaces you might end up writing lots of code that manages the lifetime of the object. I've certainly come across it in quite a few examples and tutorials.
```cpp
ISomeInterface *f_some_interface = nullptr;
f_other_interface->GetSomeInterface(&f_some_interface);

// do things to f_some_interface
// ...

// after a while you'll want to clean up
if (f_other_interface) {
	f_other_interface->Release();
    f_other_interface = nullptr;
}
```

That gets old real quick. So we'll refactor things a bit and introduce a small function that tries to hide this scaffolding.
```cpp
template <class T>
inline void com_safe_release(T **p_interface) {
    if (*p_interface) {
        (*p_interface)->Release();
        *p_interface = nullptr;
    }
}
```

The example now becomes :
```cpp
ISomeInterface *f_some_interface = nullptr;
f_other_interface->GetSomeInterface(&f_some_interface);

// do things to f_some_interface
// ...

// after a while you'll want to clean up
com_safe_release(&f_some_interface);
```

That's better right ? But it still annoying to have to remember call `com_safe_release`. It would be so much easier if the interface was released when the pointer went out of scope. Let's pull [RAII](http://en.wikipedia.org/wiki/Resource_Acquisition_Is_Initialization) into the mix and introduce a small class :
```cpp
template <class T>
class com_safe_ptr_t {
	public :
		com_safe_ptr_t(T *p_ptr = nullptr) : m_ptr(p_ptr) {}
		~com_safe_ptr_t() { com_safe_release(&m_ptr); }

		inline T **operator &() {return &m_ptr;}
		inline T * operator->() {return m_ptr;}

		inline T *get() {return m_ptr;}

	private :
		T *m_ptr;
};
```

The running example now becomes even shorter : 
```cpp
com_safe_ptr<ISomeInterface> f_some_interface = nullptr;
f_other_interface->GetSomeInterface(&f_some_interface);

// do things to f_some_interface
// ...
```
And the interface will be released when `f_some_interface` goes out of scope.

It might seem silly but I found that these small bits of code make working with COM-interfaces a lot nicer. 

"But what's the performance impact of this?" you ask. Well, the entire thing should get inlined so the performance impact should be non-existent. Even if it should come with a small performance cost, compared to the actual call to the COM-interface it will easily be negligible.

