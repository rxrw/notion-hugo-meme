---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
tags : []
slug : "{{ .Name | title }}"
description: "{{ replace .Name "-" " " | title }}"
toc: false
---
