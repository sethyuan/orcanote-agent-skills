---
name: orcanote-date
description: Guide for obtaining the date of a block in Orca Note.
---

# Block Date Extraction Guide

This skill provides instructions on how to determine the temporal context (date) of specific blocks in Orca Note.

## Procedure

1.  **Call Tool**: Use the `get_page` tool to query the page containing the target blocks.
2.  **Extract Date**: If returned page name of a block can be interpreted as a date, use it as the block's date, otherwise it means the block does not have a date.
