---
title: hexo搭建个人博客系列（三） -  设置总结
date: 2021-04-07 17:18:07
tags:
categories:
---

### 修改默认文章的模板
每次使用`hexo new "post-name"`新建一篇文章时，只有title date tags，要手动增加categories。

为了偷懒，修改了scaffolds中的post.md。

```
---
title: {{ title }}
date: {{ date }}
tags:
categories:
---

<!--more-->
```

<!--more-->
