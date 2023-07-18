- subprocess

```python
import subprocess
transid = str(subprocess.check_output(['grep' , '-w', geneid, trans2gene])).strip()
```
