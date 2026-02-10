---
name: orcanote-date
description: Guide for obtaining the date of a set of blocks in Orca Note, or a UNIX timestamp in seconds of now or today.
---

# Block Date Extraction Guide

This skill provides instructions on how to determine the temporal context (date) of specific blocks in Orca Note.

## Procedure

1.  **Call Tool**: Use the `get_page` tool to query the page containing the target blocks.
2.  **Extract Date**: If returned page name of a block can be interpreted as a date, use it as the block's date, otherwise it means the block does not have a date.

## UNIX Timestamp Guide

Use the following commands to get the required UNIX timestamp (in seconds).

### Now
Run the following command:
```bash
date +%s
```

### Today (00:00 Local Time)
Run the following command:
```bash
date -v0H -v0M -v0S +%s
```
