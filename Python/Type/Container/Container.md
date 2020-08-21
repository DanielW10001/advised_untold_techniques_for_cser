# Container

- Use Combination of List and Dictionary to create Data Structure
- Use Comprehension to access(flatten) Data Structure

```python
structured_data_set = ...
easy_access_data_set = [Expression(element) for element in structured_data_set]
```

## 1. Operation

### 1.1. Comprehension

- Create structure based on another structure: `new_structure = [<Expression>((element)+)( for element in structure)+( if <Condition>)?]`
    - Will iterate each element in `structure`
    - Each iteration's `<Expression>((element)+)` will be a element in `new_structure`
- With `if-else`: `new_structure = [<Expression>( for element in structure)+]`
    - `<Expression>`: `<PrimeryExpression>((element)+) if <Condition> else <FallBackExpression>((element)+)`
- Multiple `for-in` for nested structure:`[<Expression>(element)( for inner_structure in external_structure)* for element in inner_structure]`
    - Notice: Order
        - `for-in` that extract element from inner_structure should be after `for-in` that extract inner_structure from external_structure, which defined the inner_structure
        - `for element in inner_structure` should be after `for inner_structure in external_structure`

```python
level_0_structure =\  # nested_structure
[  # Level 0
    [  # Level 1
        [  # Level 2
            ...
        ],
        ...
        [
            ...
        ]
    ],
    ...
    [
        ...
    ]
]
flattened_structure =\
[<Expression>(element)
    for level_1_structure in level_0_structure
        for level_2_structure in level_1_structure
            ...
                for element in level_n_structure
]
invalid_flattend_structure =\
[<Expression>(element)
    for element in level_n_structure  # INVALID: level_n_structure not defined yet
        for level_n_structure in level_n-1_structure  # INVALID: level_n-1_structure not defined yet
            ...
                for level_1_structure in level_0_structure
]
```