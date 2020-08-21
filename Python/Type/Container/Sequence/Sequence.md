# Sequence

## 1. Index

- `sequence[index]`
- Access sigal element
- Distance from fist element to indicated element
    - First element's index: 0
- Can be positive or negetive

## 2. Slice

- `sequence[<InclusiveBeginIndex>: <ExclusiveEndIndex>[: <Step>]]`
- Access subsequence
- Index
    - Can be positive or negetive
    - Default: Begin or End of the sequence
    - Dummy Index
        - Greatest Index + 1
        - Representing the next position after the last element
- Step
    - If direction incompatible with bigin to end index, return empty sequence
    - Default: 1 or -1
