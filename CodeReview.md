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

Look at line 46 for an example

* Const qualification of POD data types in a function declaration has not effect. Line 75~78.

        (const size_t n, ...) <-- n is passed by value so the const does not mean anything

* Use 'nullptr' when you mean a null pointer, prefer not to use 0 (zero) or NULL. Line 75~78

        typename std::enable_if< std::is_pointer<T>::value >::type* = 0 //
        typename std::enable_if< std::is_pointer<T>::value >::type* = nullptr // Preferable

* Overriden methods should be marked as such. Line 199.

        void sanityCheck() const override;

* Use 'override' on destructors if that is what the intention is: Line 252

        ~AtomData() = default;

* const qualification of method argument: Line 639 and else where

        char const* doi <---- Not preferable
        const char* doi <---- Generally accepted

* Loops should preferably always be enclosed with braces '{}': Line 910.

        for(size_t i = 0; i < bytes; i++) crc = (crc >> 8) ^ LUT[(crc & 0xFF) ^ (*data++)];
        for(size_t i = 0; i < bytes; i++) { crc = (crc >> 8) ^ LUT[(crc & 0xFF) ^ (*data++)]; } // Preferable

* If statements should have their bodies enclosed in braces '{}': Line 938

        if(1 != fileVersion()[0] || 0 != fileVersion()[1]) throw std::runtime_error("unsupported file version");
        if(1 != fileVersion()[0] || 0 != fileVersion()[1]) {throw std::runtime_error("unsupported file version");} // Preferable

* Use static constants for file versions, offsets and other important values that do NOT change: Line 938

       1 != fileVersion()[0]// How many places is this used?
       k_FileVersion != fileVersion()[0]; // Preferable Style

* Use of old style or c-style casts: line 1070 (and elsewhere)

        noteLen() = (int16_t)str.size();
        noteLen() = static_cast<int16_t>(str.size()); // Preferable

* Use static constants for important values: Line 1147 (and below). Each Space Group should be using a constant value such as k_SG5 or something more meaningful.
