---
layout: activity
permalink: /Activities/Libraries
title: "CS374: Programming Language Principles - Static and Dynamic Linked Libraries"


info: 
  goals: 
    - To describe the linker process
    - To differentiate between static and dynamic libraries, particularly with respect to their advantages and disadvantages
  models:
    - model: |
        <img src="../images/activity-libraries/Libraries.png" alt="A flowchart indicating the linking process of several code modules with static libraries into a unified executable file, which may then load dynamic symbols via shared libraries at runtime.">
      title: Libraries
      questions:
        - "Where is the code for each static library stored for execution?  What are the advantages and disadvantages of this choice?"
        - "Where is the code for each dynamic linked library stored for execution?  What are the advantages and disadvantages of this choice?"
      embed: |
        <iframe height="400px" width="100%" src="https://repl.it/@BillJr99/DynamicMallocLibrary?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>      
        
tags:
  - libraries
  - linker
  
---

