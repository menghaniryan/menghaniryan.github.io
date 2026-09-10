---
layout: page
title: writeups
permalink: /writeups/

# This is an array that contains the categories for the coding writeup posts.
coding_options:
  categories: [writeups, coding]
  limit: 3
  show_more: true
  show_more_text: "see all..."
---

sometimes i like to solve some problems for fun, and if they're interesting i'll do a little writeup about them.

## coding questions

a collection of mainly algorithm coding questions with writeups of varying quality depending on how much i cared about it.

{% include post_list.html
    categories=page.coding_options.categories
    limit=page.coding_options.limit
    show_more=page.coding_options.show_more
    show_more_text=page.coding_options.show_more_text
%}
