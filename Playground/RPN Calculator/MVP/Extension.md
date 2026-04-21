## specify

## plan

### Agent Override

##### Extension Operator Contract Table

| Operator | Class          | Arity | Stack Effect  | Definition   | Rejection Conditions                                                      |
| -------- | -------------- | ----- | ------------- | ------------ | ------------------------------------------------------------------------- |
| `inv`    | multiplicative | 1     | pop 1, push 1 | `a -> 1/a`   | insufficient operands; zero input; non-finite result                      |
| `pow`    | power          | 2     | pop 2, push 1 | `a b -> a^b` | insufficient operands; invalid real-domain combination; non-finite result |
| `exp`    | transcendental | 1     | pop 1, push 1 | `a -> e^a`   | insufficient operands; non-finite result                                  |
| `ln`     | transcendental | 1     | pop 1, push 1 | `a -> ln(a)` | insufficient operands; non-positive input; non-finite result              |
| `abs`    | utility        | 1     | pop 1, push 1 | `a ->a`      | insufficient operands                                                     |