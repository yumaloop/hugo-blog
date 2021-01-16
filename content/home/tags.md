---
# An instance of the Tag Cloud widget.
# Docs: https://wowchemy.com/docs/page-builder/
widget: tag_cloud

# This file represents a page section.
headless: true

# Order that this section appears on the page.
weight: 3

title: <div>Popular Topics</div>
# title: '<div align="left">Tag Cloud</div>'
subtitle: ''

content:
# Choose the taxonomy from `config.toml` to display (e.g. tags, categories)
  taxonomy: tags
  # Choose how many tags you would like to display (0 = all tags)
  count: 50
design:
  # Minimum and maximum font sizes (1.0 = 100%).
  font_size_min: 0.7
  font_size_max: 2.0
  # Choose how many columns the section has. Valid values: 1 or 2.
  # Use a 1-column layout
  columns: "2"
---
