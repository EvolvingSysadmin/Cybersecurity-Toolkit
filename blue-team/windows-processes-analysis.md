---
permalink: /windows-process-analysis.html
title: Windows Process Analysis
description: 
---
<head>
<link href="css/cyber.css" rel="stylesheet">
</head>

[Toolkit Homepage](../README.md)

### Windows Process Analysis

* A parent PowerShell process spawning a child PowerShell process can be indicative of a malicious script

PowerShell: `Get-Processes` | findstr -I calc
PowerShell: `Get-Processes | findstr -I calc`
Procdump: `.procdump.exe -ma PID_Number`

Procdump creates a full memory dump for a specific process



