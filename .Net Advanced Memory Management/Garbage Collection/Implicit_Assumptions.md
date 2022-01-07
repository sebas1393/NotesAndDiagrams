[Back to Menu](../Menu.md)

# Implicit Assumptions

- Objects are either **short-lived** or **long-lived**.
- **Short-lived** objects will be allocated and discarded within a single collection cycle.
- Objects that **survive** two collection cycles are **long-lived**.
- 90% of all **small** objects are **short-lived**.
- All **large** objects (85k+) are long-lived.

- **_Do not go_** against this assumptions.

[Back to Menu](../Menu.md)