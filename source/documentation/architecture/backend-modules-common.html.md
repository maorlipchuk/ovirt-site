---
title: Backend modules common
category: architecture
authors: amuller
wiki_category: Architecture
wiki_title: Backend modules common
wiki_revision_count: 1
wiki_last_updated: 2013-02-12
---

# Backend modules common

**Introduction:** The common module is an implementation of the API used by the frontend when communicating with the backend.

**GWT Compilation:** Parts of this module is compiled to Javascript via GWT, and so special care needs to be taken as to what parts of Java are used, and what libraries. Thus, classes in this module tend to have zero or little dependencies. Furthermore, many Java classes and functions have no equivalent in Javascript and thus the GWT complains (loudly), for example: Java's String.Format.

