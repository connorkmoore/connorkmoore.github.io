---
layout: page
title: Test Robustness Improvement
description: Software-driven recovery tool reducing product returns by 51%
img: 
importance: 2
category: work
---

## Overview

Designed a software-driven recovery tool to salvage software-failed units, reducing product returns by 51% and saving ~$20k per unit in rework costs by restoring "bricked" hardware to factory state.

## Technologies Used

- Python
- C#
- SQL
- Linux

## Timeline

June 2023 - October 2024

## Impact

The solution was especially effective in internal MLAs as software reprogramming failures were contagious. A failed unit would often be disabled and subcomponents would be swapped into other units leading to a cascade of failures. The new solution was able to recover these units quickly and effectively. Each returned unit cost ~$20k and 3-6 months. Customers were very happy with the improvement.

## Technical Details

This tool automatically detects the state of the unit and recovers it back to factory-fresh software regardless of the state it was in. This led to a significant reduction in returned products as well as a reduction in time spent on rework.

Previously, operators would often have to tear apart units that failed software loading and reflash each CCA manually. This was very time-consuming and often led to further damage to the units.
