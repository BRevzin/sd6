---
title: Missing feature-test macros 2018-2019
document: D1902R0
date: today
audience: CWG, LWG
author:
    - name: Barry Revzin
      email: <barry.revzin@gmail.com>
toc: true
---

# Introduction

When [@P0941R2] was adopted both into the standard and as policy, that paper
thoroughly went through all the relevant features that existed at the time. At
the Cologne meeting in July 2019, we added signs in both the Core and Library
Working Group rooms to ensure that papers in flight did not forget - they had to
either explicitly adopt a new feature-test macro or explicitly state why they
did not need one.

In the intervening time-frame, several meetings' worth of papers were adopted
which did not have a feature-test macro. The goal is this paper is to go through
all of those papers and add the missing ones.

# 2017

## Toronto (201707)

[@P0683R1] (Default member initializers for bit-fields): no macro necessary.

[@P0704R1] (Fixing const-qualified pointers to members): no macro necessary.

[@P0409R2] (Allow lambda capture `[=, this]`): no macro necessary.

[@P0306R4] (Comma omission and comma deletion): no macro necessary.

[@P0329R4] (Designated Initialization Wording): no macro necessary. It's possible
that you could write something like:

```cpp
struct X { int first, int second; };

#if __cpp_designated_initializer
#define INIT_EQ(name) .name =
#else
#define INIT_EQ(name)
#endif

X{INIT_EQ(first) 7, INIT_EQ(second) 10};
```

That does give some benefit, in that if you get the names wrong, you'd get a
diagnostic. But would anybody actually do that?

[@P0428R2] (Familiar template syntax for generic lambdas): [this paper proposes
the macro `__cpp_familiar_template_lambda`]{.addu}. One of the things this feature
allows for is, for instance, defaulting a template parameter on a lambda,
which is arguably a feature enhancement:

```cpp
struct X { int i, j; };

auto f = 
#if __cpp_familiar_template_lambda
    []<class T=X>(T&& var)
#else
    [](auto&& var)
#endif
    {
    };
    
// having the macro allows this usage
f({1, 2});
```

[@P0702R1] (Language support for Constructor Template Argument Deduction): no
macro necessary.

[@P0734R0] (Wording Paper, C++ extensions for Concepts): [this paper proposes
the macro `__cpp_concepts`]{.addu}, which will be bumped later, but for SD-6
purposes will start here with the value 201707.

[@P0463R1] (Endian just Endian) to the C++ working paper: Already have a macro
from [@P1612R1].

[@P0682R1] (Repairing elementary string conversions): Already have a macro
from [@LWG3137].

[@P0674R1] (Extending make_shared to Support Arrays): no macro necessary.
What would you write if you didn't have the feature?

## Albuquerque (201711)

[@P0614R1] (Range-based for statements with initializer): no macro necessary.

[@P0588R1] (Simplifying implicit lambda capture): no macro necessary.

[@P0846R0] (ADL and Function Templates that are not Visible): no macro necessary.

[@P0641R2] (Resolving Core Issue #1331 (const mismatch with defaulted copy constructor)):
no macro necessary.

[@P0859R0] (Core Issue 1581: When are constexpr member functions defined?): [this
paper proposes the macro `__cpp_impl_constexpr_members_defined`]{.addu}. This issue
is a blocker for library being able to implement [@P1065R2], so the library needs
to know when to be able to do that. It's unclear if users outside of standard
library implementers will need this.

[@P0515R3] (Consistent comparison) and [@P0768R1] (Library Support for the Spaceship (Comparison) Operator): already have a macro.

[@P0857R0] (Wording for “functionality gaps in constraints”): no macro necessary.

[@P0692R1] (Access Checking on Specializations): no macro necessary.

[@P0624R2] (Default constructible and assignable stateless lambdas): no macro
necessary.

[@P0767R1] (Deprecate POD): no macro necessary.

[@P0315R4] (Wording for lambdas in unevaluated contexts): no macro necessary.

[@P0550R2] (Transformation Trait remove_cvref): [this paper proposes the macro
`__cpp_lib_remove_cvref`]{.addu}.

[@P0777R1] (Treating Unnecessary decay): no macro necessary.

[@P0600R1] (nodiscard in the Library): no macro necessary.

[@P0439R0] (Make `std::memory_order` a scoped enumeration): no macro necessary

[@P0053R7] (C++ Synchronized Buffered Ostream): [this paper proposes the macro
`__cpp_lib_syncbuf`]{.addu}.

[@P0653R2] (Utility to convert a pointer to a raw pointer): [this paper proposes
the macro `__cpp_lib_to_address`]{.addu}.

[@P0202R3] (Add `constexpr` modifiers to functions in `<algorithm>` and
`<utility>` Headers): a macro was already added.

[@P0415R1] (Constexpr for `std::complex`): [this paper proposes the macro
`__cpp_lib_constexpr_complex`]{.addu}.

[@P0718R2] (Atomic `shared_ptr`): [this paper proposes the macro
`__cpp_lib_atomic_shared_ptr`]{.addu}.

[@P0020R6] (Floating Point Atomic): [this paper proposes the macro
`__cpp_lib_atomic_float`]{.addu}.

[@P0616R0] (de-pessimize legacy `<numeric>` algorithms with `std::move`): no
macro necessary.

[@P0457R2] (String Prefix and Suffix Checking): [this paper proposes the macro
`__cpp_lib_starts_ends_with`]{.addu}.

# 2018

## Jacksonville (201803)

# Wording

Modify table 17 in 15.10 [cpp.predefined] with the following added:

::: bq
<table>
<tr>
<th>Macro Name</th>
<th>Value</th>
</tr>
<tr><td>[`__cpp_familiar_template_lambda`]{.addu}</td><td>[`201707L`]{.addu}</td></tr>
<tr><td>[`__cpp_concepts`]{.addu}</td><td>[`201707L`]{.addu}</td></tr>
<tr><td>[`__cpp_impl_constexpr_members_defined`]{.addu}</td><td>[`201711L`]{.addu}</td></tr>
</table>
:::

Modify table 36 in 17.3.1 [support.limits.general] with the following added:

::: bq
<table>
<tr>
<th>Macro Name</th>
<th>Value</th>
<th>Header(s)</th>
</tr>
<tr><td>[`__cpp_lib_remove_cvref`]{.addu}</td><td>[`201711L`]{.addu}</td><td>[`<type_traits>`]{.addu}</td></tr>
<tr><td>[`__cpp_lib_syncbuf`]{.addu}</td><td>[`201711L`]{.addu}</td><td>[`<syncstream>`]{.addu}</td></tr>
<tr><td>[`__cpp_lib_to_address`]{.addu}</td><td>[`201711L`]{.addu}</td><td>[`<memory>`]{.addu}</td></tr>
<tr><td>[`__cpp_lib_constexpr_complex`]{.addu}</td><td>[`201711L`]{.addu}</td><td>[`<complex>`]{.addu}</td></tr>
<tr><td>[`__cpp_lib_atomic_shared_ptr`]{.addu}</td><td>[`201711L`]{.addu}</td><td>[`<atomic>`]{.addu}</td></tr>
<tr><td>[`__cpp_lib_atomic_float`]{.addu}</td><td>[`201711L`]{.addu}</td><td>[`<atomic>`]{.addu}</td></tr>
<tr><td>[`__cpp_lib_starts_ends_with`]{.addu}</td><td>[`201711L`]{.addu}</td><td>[`<string> <string_view>`]{.addu}</td></tr>
</table>
:::