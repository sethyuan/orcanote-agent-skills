---
name: orcanote-template
description: Guide for creating reusable templates in Orca Note with optional dynamic date and time variables.
---

# Orca Note Templates

This skill provides a standard workflow for creating and managing reusable block templates within Orca Note.

## Template Creation Workflow

To create a new template, follow these sequential steps:

1.  **Create the Page**: Use the `create_page` tool to create a new page that will serve as your template.
2.  **Tag as Template**: Add the tag `Template` using the `insert_tags` tool to the newly created page. This tag is essential for Orca Note to recognize and list the page as a template.
3.  **Define Content**: Insert the desired structure and boilerplate text into the page.

## Dynamic Template Variables

Orca Note templates support dynamic variables that resolve automatically when new blocks are generated from the template.

### Date Variables

Insert date placeholders using the following syntax: `<% date:natural_language_description %>`

**Examples:**
- Today's date: `<% date:today %>`
- Specific day: `<% date:Friday %>`
- Relative date: `<% date:next Monday %>`

### Time Variables

Insert time placeholders using the following syntax: `<% time:natural_language_description[:format] %>`

The `format` parameter is optional. If provided, use standard time format strings (e.g., `HH:mm`).

**Examples:**
- Current time: `<% time:now %>`
- Custom format: `<% time:one hour later:HH:mm %>`
- Relative time: `<% time:tomorrow morning %>`

## Usage Example

When asked to "create a daily log template", you should:

1. Call `create_page(name="Daily Log Template")`
2. Apply the `Template` tag by calling the `insert_tags` tool.
3. Add content such as:
   ```markdown
   # Log for <% date:today %>
   Start Time: <% time:now:HH:mm %>
   ## Tasks
   - ...
   ```
