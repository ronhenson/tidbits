---
title: "Properties Template"
created: 2021-07-27 14:21:20
tags: markdown
keywords: markdown, template, foam
---

# Properties Template

```markdown
${text:---}
title: "${TM_FILENAME_BASE/([^-]+)(-*)/${1:/capitalize}${2:+ }/g}"
created: ${CURRENT_YEAR}-${CURRENT_MONTH}-${CURRENT_DATE} ${CURRENT_HOUR}:${CURRENT_MINUTE}:${CURRENT_SECOND}
tags: template
keywords: markdown, template
---

# ${TM_FILENAME_BASE/([^-]+)(-*)/${1:/capitalize}${2:+ }/g}
```
