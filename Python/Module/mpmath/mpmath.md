# mpmath: math precision

`pip install mpmath`

```python3
import mpmath
mpmath.mp.dps = DPS  # Decimal Places
float_num = mpmath.mpf(VALUE|'VALUE')  # mp float
float_num OPERATOR mpmath.mpf(VALUE)  # Use VALUE directly will lead to type(VALUE) result
result = mpmath.power(BASE, EXPONENT)
```
