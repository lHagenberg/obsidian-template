---
title: "{{title}}"
authors: "{{authors}}"
citekey: "{{citekey}}"
year: '{{date | format("YYYY")}}'
tags: []
study-area:
rating: "?"
type:
status: unread
imported: '{{importDate | format("YYYY-MM-DD")}}'
---
- **Document**:: [PDF]({{desktopURI}})
- **Link**:: {{url}}

# Abstract
````
{{abstractNote}}
````

# Notes 
- [a] 


**Key assumptions made**


# Connections
**Cited papers of interest**


# Annotations & Highlights
{% for annotation in annotations %}{% if annotation.annotatedText %} - {{annotation.annotatedText}} *(p. {{annotation.page}})*{% if annotation.comment %} ^{{annotation.comment}}{% endif %} 
{% endif %}{% endfor %}
