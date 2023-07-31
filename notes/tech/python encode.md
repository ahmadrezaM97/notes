

```python
a = "احمدرضا"
a.encode('utf-8')
# b'\xd8\xa7\xd8\xad\xd9\x85\xd8\xaf\xd8\xb1\xd8\xb6\xd8\xa7'
a.encode('unicode_escape')
# b'\\u0627\\u062d\\u0645\\u062f\\u0631\\u0636\\u0627'

```