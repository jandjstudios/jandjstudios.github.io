---
permalink: /test/
title: "test"
excerpt: "A post with custom sidebar content."
author_profile: false
sidebar:
  - title: "Title"
    image: http://placehold.it/350x250
    image_alt: "image"
    text: "Some text here."
  - title: "Another Title"
    text: "More text here."
    nav: sidebar-sample
---

This post has a custom sidebar set in the post's YAML Front Matter.

An example of how that YAML could look is:

```yaml
sidebar:
  - title: "Title"
    image: http://placehold.it/350x250
    image_alt: "image"
    text: "Some text here."
  - title: "Another Title"
    text: "More text here."
    nav: sidebar-sample
```