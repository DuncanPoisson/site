---
title: "Example Research Entry"
date: 2026-04-15
draft: false
tags: ["complexity", "simulation"]
categories: ["research-notes"]
description: "A placeholder research entry showing frontmatter conventions, code blocks, and cross-links."
---

<!--
  Authoring note — Cross-linking between sections:
  Use the Hugo ref shortcode to link to other content pages, e.g.:
    [Link text]( { {< ref "/journal/some-entry" >} } )   (remove spaces around braces)
  Hugo validates ref targets at build time; broken links fail the build.
  Prefer ref (absolute URL) over relref (relative) for links that
  cross section boundaries (e.g., journal ↔ blog).
-->

[placeholder] This entry demonstrates the structure of a research journal entry.

## Background

[placeholder] Describe the motivation and context for this piece of work. Agent-based models of urban growth often exhibit emergent complexity that is difficult to characterize with summary statistics alone.

## Method

[placeholder] Document the approach, tools, and parameters used.

```python
import numpy as np

# Example: simple random-walk proxy for urban spread
steps = np.random.choice([-1, 1], size=(1000, 2))
positions = np.cumsum(steps, axis=0)
print(f"Final displacement: {np.linalg.norm(positions[-1]):.2f}")
```

## Observations

[placeholder] Record findings, plots, and intermediate results.

![Placeholder figure — replace with actual output](data:image/svg+xml,%3Csvg%20xmlns='http://www.w3.org/2000/svg'%20width='400'%20height='200'%3E%3Crect%20width='400'%20height='200'%20fill='%23eee'/%3E%3Ctext%20x='50%25'%20y='50%25'%20dominant-baseline='middle'%20text-anchor='middle'%20fill='%23999'%3ETODO:%20figure%3C/text%3E%3C/svg%3E)

## Related

See the companion blog post: [Example Post]({{< ref "/blog/example-post" >}}).
