---
# Leave the homepage title empty to use the site title
title: ''
summary: ''
date: 2026-01-05
type: landing

design:
  # Default section spacing
  spacing: '0'

sections:
  # Developer Hero - Gradient background with name, role, social, and CTAs
  - block: dev-hero
    id: hero
    content:
      username: me
      greeting: "Hi, I'm"
      show_status: true
      show_scroll_indicator: true
      typewriter:
        enable: true
        prefix: "Skills in"
        strings:
          - "Governance, Risk & Compliance"
          - "eDiscovery & Legal Tech"
          - "Identity & Cloud Security"
          - "Risk-Aware Security Operations"
        type_speed: 70
        delete_speed: 40Foresnsics
        pause_time: 2500
      cta_buttons:
        - text: View My Work
          url: "#projects"
          icon: arrow-down
        - text: Get In Touch
          url: "#contact"
          icon: envelope
    design:
      style: centered
      avatar_shape: circle
      animations: true
      background:
        color:
          light: "#fafafa"
          dark: "#0a0a0f"
      spacing:
        padding: ["6rem", "0", "4rem", "0"]
  
  # Filterable Portfolio - Alpine.js powered project filtering
  - block: portfolio
    id: projects
    content:
      title: "Featured Projects"
      subtitle: "A selection of my recent security and risk projects"
      count: 5
      filters:
        folders:
          - projects
      buttons:
        - name: All
          tag: '*'
        - name: GRC & Risk Management
          tag: GRC  
        - name: eDiscovery & Legal Tech
          tag: eDiscovery
        - name: Cloud & Identity Security
          tag: Cloud
        - name: Digital Forensics
          tag: Forensics


      default_button_index: 0
      # Archive link auto-shown if more projects exist than 'count' above
      # archive:
      #   enable: false  # Set to false to explicitly hide
      #   text: "Browse All"  # Customize text
      #   link: "/work/"  # Custom URL
    design:
      columns: 3
      background:
        color:
          light: "#ffffff"
          dark: "#0d0d12"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]
  
 
  # Visual Security Stack - Icons organized by category
  - block: tech-stack
    id: skills
    content:
      title: "Security & Technology Stack"
      subtitle: "DFIR, eDiscovery, GRC, and Security Awareness & Risk Context"
      categories:
      - name: Blue Team & Incident Response
        items:
          - name: Windows Security & Event Analysis
            icon: devicon/windows11
          - name: Linux Security
            icon: devicon/linux
          - name: Network Traffic Analysis (Wireshark)
            icon: custom/wireshark
          - name: Incident Response 
            icon: custom/incident

      - name: GRC & Identity Security
        items:
          - name: Risk Management & GRC
            icon: custom/risk
          - name: Identity & Access Management (IAM)
            icon: custom/identity
          - name: Data Protection & DLP
            icon: custom/data
          - name: Cloud Security Fundamentals (Azure)
            icon: devicon/azure

      - name: eDiscovery & Legal Technology
        items:
          - name: Relativity
            icon: custom/relativity
          - name: Microsoft Purview eDiscovery
            icon: custom/purview
          - name: Legal Hold & Retention
            icon: custom/legal
          - name: Evidence Handling & Documentation
            icon: custom/document

      - name: Programming & Automation
        items:
          - name: Python (Security Scripting)
            icon: devicon/python
          - name: SQL / MySQL
            icon: devicon/mysql
          - name: PowerShell Automation
            icon: devicon/powershell
          - name: Git & GitHub
            icon: brands/github
          - name: C# Development 
            icon: devicon/csharp

      - name: Security Awareness & Risk Context
        items:
          - name: Network Reconnaissance (Nmap)
            icon: custom/nmap
          - name: Web Application Testing (Burp Suite)
            icon: custom/burpsuite
          - name: Kali Linux (Lab Environment)
            icon: custom/kali

    design:
      style: grid
      columns: 4
      show_levels: false
      background:
        color:
          light: "#f5f5f5"
          dark: "#08080c"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]

  # Certifications Section (Single Line)
  - block: tech-stack
    id: certifications
    content:
      title: "Certifications"
      subtitle: "Industry-recognised credentials"
      categories:
        - name: Certifications
          items:
            - name: "Security+"
              icon: custom/comptia
            - name: "Blue Team Level 1 (BTL1)"
              icon: custom/shield
            - name: "Microsoft SC-900"
              icon: devicon/azure
            - name: "Purview (Retention & eDiscovery)"
              icon: custom/purview
            - name: "Relativity Certified Pro"
              icon: custom/relativity
            - name: "Google Cybersecurity Specialization"
              icon: devicon/google
            - name: "OSCP (In progresss)"
              icon: custom/oscp

    design:
      style: grid
      show_levels: false
      background:
        color:
          light: "#ffffff"
          dark: "#0d0d12"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]

  
  # Experience Timeline
  - block: resume-experience
    id: experience
    content:
      title: Experience
      date_format: Jan 2006
      items:
        - title: Senior Software Engineer
          company: Tech Corp
          company_url: ''
          company_logo: ''
          location: San Francisco, CA
          date_start: '2023-01-01'
          date_end: ''
          description: |2-
            * Lead development of microservices architecture serving 1M+ users
            * Improved API response time by 40% through optimization
            * Mentored team of 5 junior developers
            * Tech stack: React, Node.js, PostgreSQL, AWS
        - title: Full-Stack Developer
          company: Startup Inc
          company_url: ''
          company_logo: ''
          location: Remote
          date_start: '2021-06-01'
          date_end: '2022-12-31'
          description: |2-
            * Built and deployed 3 production applications from scratch
            * Implemented CI/CD pipeline reducing deployment time by 60%
            * Collaborated with design team on UI/UX improvements
            * Tech stack: Next.js, Express, MongoDB, Docker
        - title: Junior Developer
          company: Web Agency
          company_url: ''
          company_logo: ''
          location: New York, NY
          date_start: '2020-01-01'
          date_end: '2021-05-31'
          description: |2-
            * Developed client websites using modern web technologies
            * Maintained and updated legacy codebases
            * Participated in code reviews and agile ceremonies
            * Tech stack: React, WordPress, PHP, MySQL
    design:
      columns: '1'
      background:
        color:
          light: "#f5f5f5"
          dark: "#08080c"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]
  
  
  - block: collection
    id: writeups
    content:
      title: Recent Posts
      subtitle: 'Writeups of my recent labs, experiences and more'
      text: ''
      filters:
        folders:
          - writeups
        exclude_featured: false
      count: 6
      order: desc
      design:
      view: card
      columns: 3
      background:
        color:
          light: "#f5f5f5"
          dark: "#08080c"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]
  
  # Contact Section
  - block: contact-info
    id: contact
    content:
      title: Get In Touch
      subtitle: 
      text: |-
          I am currently seeking cybersecurity graduate roles or early-career opportunities. Whether you are looking to hire, collaborate, or just want to say hi, feel free to reach out!
      email: antoinemindaoudou@gmail.com
      autolink: true
    design:
      columns: '1'
      background:
        color:
          light: "#ffffff"
          dark: "#0d0d12"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]
  
  # CTA Card
  - block: cta-card
    content:
      title: "Open to Opportunities"
      text: |-
        I'm currently seeking **graduate cybersecurity roles**, internships, or early-career opportunities in **GRC, Forensics, Cloud Security, or Blue Team operations**.
      button:
        text: 'Download Resume'
        url: uploads/resume.pdf
        new_tab: true
    design:
      card:
        # Light mode: soft pastel theme gradient | Dark mode: rich deep gradient
        css_class: 'bg-gradient-to-br from-primary-200 via-primary-100 to-secondary-200 dark:from-primary-600 dark:via-primary-700 dark:to-secondary-700'
        text_color: dark
      background:
        color:
          light: "#f5f5f5"
          dark: "#08080c"
      spacing:
        padding: ["4rem", "0", "6rem", "0"]
---
