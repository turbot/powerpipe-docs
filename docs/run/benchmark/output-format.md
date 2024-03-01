---
title: Formatting Output
sidebar_label: Formatting Output
---


# Formatting Output
By default, Powerpipe shows a progress bar and produces colorized output to the console screen.  Powerpipe provides many options to control the output formatting.

If you run Powerpipe from a CI tool or batch scheduler, you may want to use non-colorized output and disable the progress bar:

```bash
powerpipe benchmark run cis_v150 --output=plain --progress=false
```

Some benchmarks are quite verbose.  To show only the items that are in alarm or error, use `brief` output:
```bash
powerpipe benchmark run cis_v150 --output=brief
```

You can also export the full output to JSON:
```bash
powerpipe benchmark run cis_v150 --export=json
```

Or CSV:
```bash
powerpipe benchmark run cis_v150 --export=csv
```

Or markdown:
```bash
powerpipe benchmark run cis_v150 --export=md
```

Or HTML:
```bash
powerpipe benchmark run cis_v150 --export=html
```

Or export to multiple formats from a single run:
```bash
powerpipe benchmark run cis_v150 --export=csv --export=json --export=html
```

You can export to a filename of your choosing - Powerpipe will infer the output type by the file extension:
```bash
powerpipe benchmark run cis_v150 --export=output.csv --export=output.json --export=output.md
```

You can also send JSON output to STDOUT if you want to redirect it to a file or pipe it to another program:
```bash
powerpipe benchmark run cis_v150 --progress=false --output=json | jq
```



<!--
***[TODO:  where to put this page....]***
Powerpipe even allows you to [write your own control output templates!](develop/writing-control-output-templates).



-->