# json

```python3
# Python object -> json string
jsonstr = json.dumps(object[,  # Dump to string
                     skipkeys=True][,
                     ensure_ascii=False][,
                     allow_nan=False][,
                     indent=4][,
                     separators=(',', ':')][,
                     default=lambda unserializable_object: serializable_value][,
                     sort_keys=True][,
                     cls=CustomeEncoder])
json.dump(object, outfile[,  # Dump to file
          skipkeys=True][,
          ensure_ascii=False][,
          allow_nan=False][,
          indent=4][,
          separators=(',', ':')][,
          default=lambda unserializable_object: serializable_value][,
          sort_keys=True][,
          cls=CustomeEncoder])
json_encoder = json.JSONEncoder([skipkeys=True][,
                                ensure_ascii=False][,
                                allow_nan=False][,
                                indent=4][,
                                separators=(',', ':')][,
                                default=lambda unserializable_object: serializable_value][,
                                sort_keys=True])
serializable_value = json_encoder.default(unserializable_object)  # Serializable
jsonstr = json_encoder.encode(object)
jsonstr_iterable = json_encoder.iterencode(object)  # jsonstr_iterable: Iterable over each part of JSON string
class CustomEncoder(json.JSONEncoder):
    def default(self, unserializable_object):
        if condition(unserializable_object):
            return serializable_value
        return super().default(unserializable_object)  # Which should raise TypeError for other conditions

# json string -> Python object
object = json.loads(jsonstr[,  # Load from string
                    cls=CustomDecoder][,
                    object_hook=lambda dictionary: value][,
                    object_pairs_hook=lambda list_of_key_value_pairs: value][,  # list_of_key_value_pairs: [(key, value)(, (key, value))*]; object_pairs_hook takes priority over object_hook
                    parse_float=lambda float_string: value][,
                    parse_int=lambda int_string: value][,
                    parse_constant=lambda constant_string: value])
object = json.load(infile[,  # Load from file
                   cls=CustomDecoder][,
                   object_hook=lambda dictionary: value][,
                   object_pairs_hook=lambda list_of_key_value_pairs: value][,  # list_of_key_value_pairs: [(key, value)(, (key, value))*]; object_pairs_hook takes priority over object_hook
                   parse_float=lambda float_string: value][,
                   parse_int=lambda int_string: value][,
                   parse_constant=lambda constant_string: value])
json_decoder = json.JSONDecoder([object_hook=lambda dictionary: value][,
                                object_pairs_hook=lambda list_of_key_value_pairs: value][,  # list_of_key_value_pairs: [(key, value)(, (key, value))*]; object_pairs_hook takes priority over object_hook
                                parse_float=lambda float_string: value][,
                                parse_int=lambda int_string: value][,
                                parse_constant=lambda constant_string: value][,
                                strict=False])  # Allows escape character
object = json_decoder.decode(jsonstr)
object, end_index = json.decoder.raw_decode(jsonstr)
class CustomDecoder(json.JSONDecoder):
    # ...
```

```bash
echo 'python_object' | python -m json.tool  # Encode from CLI
python -m json.tool[ -h|--help][ --sort-keys][ --json-lines][ infile][ outfile]  # Encode
```
