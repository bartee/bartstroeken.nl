+++ 
draft = true
date = 2024-06-18T20:57:59+02:00
title = "Help, my dependencies ate my error budget!"
description = "With error budgets, if set up correctly, you monitor how well you're doing what your users can expect from you. But what if the problem's upstream? This blog can give you some insights in how to tackle that. "
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++

- Error budgets are a uniform quality metric. See https://bolcom.sharepoint.com/:w:/r/sites/sre/Gedeelde%20documenten/General/Theoretical/reporting_on_reliability_with_errorbudgets.docx?d=wa9dda93f45be411281f7f3e74ffd0ec6&csf=1&web=1&e=hbvSth
- Error budget burn can be outside of your scope
- you can monitor that yourself by measuring circuit breaker state
    - wat is effect van combinatie-SLO van jouw edge wanneer de CB open is? Technisch kan dat, maar dat is voor je eindgebruiker niet relevant.
- Step 1: Separate SLI on circuit breaker state
- Step 2: Use CB SLI to urge dependency to set up SLI on that.