---
layout: project-post
title: "Writing a Modern Python Framework: Purpose, Values, and Goals"
slug: purpose-values-goals
description: "Why write a Python framework in 2020?"
date: 2020-05-10
author: Franco Posa
order_number: 1
---

I have some existing boilerplate code I commonly use to stitch together my preferred libraries into async Python API services, and it has finally reached the point where it has become laborious to copy the code into new projects and keep them updated with my latest version.

Naturally, this leads to the idea that this code should be a library. It's Python, so the library should be on PyPI. If the library is on PyPI, the code will be public. If the code is public, my name will be associated it. Finally, if a Python library is running around in the wild with my name on it, I would prefer that it is a good one - or at least that it does not competely suck.

Creating a web framework is a pretty vague and daunting task, particularly if you want it not to suck! So I thought it may be useful to sketch out a vision for what I am really hoping to accomplish.

* **Purpose**: Why am I writing a Python framework at all?
* **Values**: What guiding principles or philosophies will drive the design decisions?
* **Goals**: What would a successful outcome of this project look like?
