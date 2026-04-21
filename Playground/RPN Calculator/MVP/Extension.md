---
url: https://chatgpt.com/g/g-p-69e6210469388191b8880a8407594f1a/c/69e6254f-c084-83eb-a38f-908afc14da4d
---

## specify

## plan

### Agent Override

##### Real-Domain Policies - `pow`

| Base | Exponent | Validity            |
| ---- | -------- | ------------------- |
| <0   | ∉ ℤ      | rejected as invalid |
| 0    | <0       | rejected as invalid |
| 0    | 0        | rejected as invalid |

##### Extension Operator Contract Table

| Operator | Class          | Arity | Stack Effect  | Definition   | Rejection Conditions                                                      |
| -------- | -------------- | ----- | ------------- | ------------ | ------------------------------------------------------------------------- |
| `inv`    | multiplicative | 1     | pop 1, push 1 | `a -> 1/a`   | insufficient operands; zero input; non-finite result                      |
| `pow`    | power          | 2     | pop 2, push 1 | `a b -> a^b` | insufficient operands; invalid real-domain combination; non-finite result |
| `exp`    | transcendental | 1     | pop 1, push 1 | `a -> e^a`   | insufficient operands; non-finite result                                  |
| `ln`     | transcendental | 1     | pop 1, push 1 | `a -> ln(a)` | insufficient operands; non-positive input; non-finite result              |
| `abs`    | utility        | 1     | pop 1, push 1 | `a ->a`      | insufficient operands                                                     |