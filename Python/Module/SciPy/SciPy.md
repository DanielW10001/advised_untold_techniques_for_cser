# SciPy

`pip install numpy matplotlib`

```python3
import numpy
import matplotlib.pyplot

xlist = numpy.arange(FROM, TO, STEP[, dtype=object|numpy.int64|numpy.float64|numpy.uint64_t])
xlist = numpy.linspace(FROM, TO, COUNT[, dtype=object|numpy.int64|numpy.float64|numpy.uint64_t])
xlist = []  # Use specific external type, like decimal.Decimal
x = FROM
while x <= TO:
    xlist.append(x)
    x += STEP

ylist = [EXPRESSION(x) for x in xlist]

# $LATEX$; `usetex=True` for tex environment
matplotlib.pyplot.title(r"TITLE")
matplotlib.pyplot.xlabel(r"XLABEL")
matplotlib.pyplot.ylabel(r"YLABEL")
matplotlib.text(X, Y, CONTENT)
{matplotlib.pyplot.plot(xlist, ylist, label=r"LABEL")}
matplotlib.pyplot.xlim(FROM, TO)
matplotlib.pyplot.ylim(FROM, TO)
matplotlib.pyplot.legend()

matplotlib.pyplot.show()
matplotlib.pyplot.gcf()  # Get current figure
matplotlib.pyplot.savefig('<FileDir>/<Figure>.png')

# numpy
import numpy
num_list = numpy.arange(FROM, TO, STEP[, dtype=object|numpy.int64|numpy.float64|numpy.uint64_t])
num_list = numpy.linspace(FROM, TO, COUNT[, dtype=object|numpy.int64|numpy.float64|numpy.uint64_t])

# matplotlib
# $LATEX$
# matplotlib.pyplot
import matplotlib.pyplot
matplotlib.pyplot.title(r"TITLE")
matplotlib.pyplot.xlabel(r"XLABEL")
matplotlib.pyplot.ylabel(r"YLABEL")
{matplotlib.pyplot.plot(xlist, ylist, label=r"LABEL")}
matplotlib.pyplot.xlim(FROM, TO)
matplotlib.pyplot.ylim(FROM, TO)
matplotlib.pyplot.legend()

matplotlib.pyplot.show()
matplotlib.pyplot.gcf()  # Get current figure
matplotlib.pyplot.savefig('<FileDir>/<Figure>.png')
```

## 1. Install

```powershell
pipenv install numpy scipy matplotlib ipython jupyter pandas sympy nose
```

## 2. 绘制函数图像

```python
xList = numpy.arange(<From>, <End>, <Step>)
xList = numpy.linspace(<From>, <End>, <Count>)
yList = Expression(xList)
yList = [Expression(i) for i in x]
yList = (math.e)**xList  # y = e^x
yList = numpy.sin(xList) # y = sin(x)

matplotlib.pyplot.title("<Diagram Title>")  # $LATEX$
matplotlib.pyplot.plot(xList, yList, label="<Label>")
matplotlib.pyplot.xlabel("<XLabel>")
matplotlib.pyplot.ylabel("<YLabel>")
matplotlib.pyplot.legend()

matplotlib.pyplot.show()
matplotlib.pyplot.gcf()
matplotlib.pyplot.savefig('<FileDir>/<Figure>.png')
```

## 3. `numpy` Module

### 3.1. Method

#### 3.1.1. `arange()`

```python
numList = numpy.arange(<From>, <End>, <Step>)
```

#### 3.1.2. `linspace()`

```python
numList = numpy.linspace(<From>, <End>, <Count>)
```

#### 3.1.3. `sin()`

```python
numList = numpy.linspace(<From>, <End>, <Count>)
yList = numpy.sin(xList) # y = sin(x)
```

## 4. `matplotlib` Module

- Matplotlib Plotting Cookbook
- Support LaTeX in label string: `"$<LaTeX>$"`

### 4.1. `pyplot` Module

- [Reference Method List](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.html#module-matplotlib.pyplot)

#### 4.1.1. Method

##### 4.1.1.1. `title()`

- Title the diagram

```python
matplotlib.pyplot.title("<Diagram Title>")
```

##### 4.1.1.2. `plot()`

- Generate diagram on current figure
- Invoke multiple times on one figure will generate multiple diagram on one figure

```python
matplotlib.pyplot.plot(xList, yList, label="<Label>")
```

##### 4.1.1.3. `legend()`

- Place a legend in figure

##### 4.1.1.4. `show()`

- Show diagram with GUI

```python
matplotlib.pyplot.show()
```

##### 4.1.1.5. `draw()`

- Redraw the current figure

```python
pyplot.draw()
```

##### 4.1.1.6. `figure()`

- Generate empty figure

```python
pyplot.figure(figsize=(figure_width, figure_height))
```

##### 4.1.1.7. `xlim()`, `ylim()`

- Set limitation of x or y demension

```python
pyplot.xlim(start_limitation, end_limitation)
pyplot.ylim(start_limitation, end_limitation)
```

##### 4.1.1.8. `xlabel()`, `ylabel()`

- Set label of x or y demension

```python
pyplot.xlabel("x")
pyplot.ylabel("y")
```

##### 4.1.1.9. `xticks()`, `yticks()`

- Set ticks

```python
pyplot.xticks([tick, tick, tick, ..., tick])
pyplot.yticks([tick, tick, tick, ..., tick], [corespondent_label, corespondent_label, ..., corespondent_label])
```

##### 4.1.1.10. `savefig()`

- Save current figure

```python
pyplot.savefig("<FileDir>")  # FilrDir can be <Dir>/<Name>.png
```

##### 4.1.1.11. `gcf()`

- Get current figure object
- If no current figure exists, create one with `figure()`

```python
pyplot.gcf()
```

### 4.2. `figure` Module

#### 4.2.1. `Figure` Class

##### 4.2.1.1. Method

###### 4.2.1.1.1. `savefig()`

- Save current figure

```python
figure.savefig("<FileDir>")  # FilrDir can be <Dir>/<Name>.png
```
