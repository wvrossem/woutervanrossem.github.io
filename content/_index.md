---
# Leave the homepage title empty to use the site title
title: ''
date: 2024-01-08
type: landing

design:
  # Default section spacing
  spacing: '6rem'

sections:
  - block: resume-biography-3
    content:
      # Choose a user profile to display (a folder name within `content/authors/`)
      username: admin
      text: ''
      # Show a call-to-action button under your biography? (optional)
      # button:
      #   text: Download CV
      #   url: uploads/resume.pdf
      headings:
        about: ''
        education: ''
        interests: ''
    design:
      # Apply a gradient background
      css_class: hbx-bg-gradient
      # Avatar customization
      avatar:
        size: large # Options: small (150px), medium (200px, default), large (320px), xl (400px), xxl (500px)
        shape: circle # Options: circle (default), square, rounded
  - block: resume-skills
    content:
      title: "Skills"
      text: ""
      # Choose a user to display skills from (a folder name within `content/authors/`)
      username: "admin"
    design:
      columns: '1'
  - block: collection
    id: publications
    content:
      title: Featured Publications
      filters:
        folders:
          - publication
        featured_only: true
    design:
      view: article-grid
      columns: 2
      show_read_time: false
  - block: collection
    content:
      title: Recent Publications
      filters:
        folders:
          - publication
        exclude_featured: false
    design:
      columns: '3'
      view: citation
  - block: collection
    id: projects
    content:
      title: Selected Projects
      text: Here are a selection of projects that I have worked on over the years.
      filters:
        folders:
          - project
    design:
      view: article-grid
      fill_image: true
      columns: 2
      show_date: false
      show_read_time: false
      show_read_more: false

  - block: collection
    id: events
    content:
      title: Talks & Events
      filters:
        folders:
          - event
    design:
      view: article-grid
      fill_image: true
      columns: 2
      show_date: false
      show_read_time: false
      show_read_more: false
---
