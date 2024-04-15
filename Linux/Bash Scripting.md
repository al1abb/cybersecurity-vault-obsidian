Bash scripts always start with a shebang
`!#/bin/bash`

For loop syntax
```bash
!#/bin/bash

for i in {0000..9999}
do
    echo Hello World
done
```

If the variable is looped over a specific range, it might be easier to only write the bounds. For example, if we want to loop over 1-10, the syntax would be `{0..10}`. If we additionally want every number to have two digits (00, 01, 02, … 10), we can add the zero digit to the number `{00..10}`.