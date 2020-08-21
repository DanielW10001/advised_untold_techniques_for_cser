# csv: Comma Separated Values parser

```python3
# Read
with open(filedir, newline='') as csvfile:
    reader = csv.reader(csvfile|[rowstr(, rowstr)*][, dialect='dialect'|custom_dialect][, **fmtparams])
    for row in reader:  # `row` is list of field strings
        # ...
    row = next(reader)
    reader.dialect  # Read-only dialect description
    reader.line_num  # Number of read lines

    reader = csv.DictReader(csvfile|[rowstr(, rowstr)*][, fieldnames=['fieldname'(, 'fieldname')*]][, restkey='restkey'][, restval=value][, dialect='dialect'|custom_dialect][, **fmtparams])
    for row in reader:  # `row` is dict from names to values
        row['name']
    row = next(reader)
    reader.dialect  # Read-only dialect description
    reader.line_num  # Number of read lines
    reader.fieldnames

# Write
with open(filedir, 'w', newline='') as csvfile:
    writer = csv.writer(csvfile[, dialect='dialect'|custom_dialect][, **fmtparams])
    writer.writerow([field(, field)*])
    writer.writerows([row(, row)*])
    writer.dialect  # Read-only dialect description

    writer = csv.DictWriter(csvfile, fieldnames=['fieldname'(, 'fieldname')*][, restval=value][, extrasaction='ignore'][, dialect='dialect'|custom_dialect][, **fmtparams])
    writer.writeheader()
    writer.writerow({'name': 'value'(, 'name': 'value')*})
    writer.writerows([row(, row)*])

# Dialect
class CustomDialect(csv.Dialect):
    delimiter = ','
    doublequote = True|False  # True: ""; False: \"
    escapechar = None|'\\'
    lineterminator = '[\r][\n]'
    quotechar = '"'
    quoting = csv.QUOTE_ALL|csv.QUOTE_MINIMAL|csv.QUOTE_NONNUMERIC|csv.QUOTE_NONE
    skipinitialspace = True|False
    strict = True|False
csv.register_dialect('name'[, dialect='dialect'|custom_dialect][, **fmtparams])
csv.unregister_dialect('name')
dialect = csv.get_dialect('name')
name_strs = csv.list_dialects()  # Predefined: 'excel', 'ecel-tab', 'unix'
old_limit = csv.field_size_limit([new_limit])
with open(filedir, newline='') as csvfile:
    sniffer = csv.Sniffer()
    dialect = sniffer.sniff(csvfile.read()[, delimiters='delimiters'])
    sniffer.has_header(csvfile.read())  # Check if has header
```
