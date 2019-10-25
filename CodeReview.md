# SHTfile  Code Review #

Over all a good piece of code and reasonably well written but there are some maintainability
issues that should be addressed before general consumption by a larger community
is established.

## Formatting ##

While the file is formatted with an eye towards presentation rather than adhering to 
any standard. The use of a .clang-format file will save future maintainers from having
to tediously follow an arbitrary and undoumented formatting scheme.

* Modern C++ eschews the use of structures like the following:

        char Something[8] = ".........."; // Not typical in modern C++
        std::array<char, 8> = { "........" };

* Use 'nullptr' when you mean a null pointer, prefer not to use 0 (zero) or NULL.

        typename std::enable_if< std::is_pointer<T>::value >::type* = 0 //
        typename std::enable_if< std::is_pointer<T>::value >::type* = nullptr // Preferable

* Overriden methods should be marked as such.

        void sanityCheck() const override;

* Use 'override' on destructors if that is what the intention is

        ~AtomData() = default;

* Use of old style or c-style casts

        noteLen() = (int16_t)str.size();
        noteLen() = static_cast<int16_t>(str.size()); // Preferable

