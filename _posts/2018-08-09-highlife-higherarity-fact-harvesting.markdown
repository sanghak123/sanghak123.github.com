---
layout: post
title: "HighLife: Higher-arity Fact Harvesting 요약"
date: "2018-08-09 15:05:49 +0900"
---
# HighLife: Higher-arity Fact Harvesting

## ABSTACT
Traditional methods: binary relations only  
HighLife's approach: __higer-arity__ relation that can handle __partially__ observed facts

## INTRODUCTION
### Motivation and Problem
Limitation of common KBs: binary relations only (SPO triples)  
Converting higher-arity to binary is not always possible. (because of loss of information)
### State of the Art and its Limitations
IE (Information Extraction) methods in [21, 24]: large number of ungeneralizable rules  
Method in [20]: small pattern collection and manual curation  
SRL (Semantic Role Labeling): based on constrianed learning, using fine-grained syntatic and lexical features, used as baseline for HighLife  
Previous distant supervision approach: only binary facts
### Approach and Constribution
seed fact --(distant supervision)--> pattern  
text --(pattern)--> fact candidates  
fact candidates --(MaxSat-based reasoner)--> non-spurious fact candidates  
Key difficulty: partially expressed higer-arity facts  
Appication: i) health-related texts ii) news articles  
Comparison to SRL system baseline

## RELATED WORK
### Knowledge Bases
N-ary facts from pre-structured resources
### Open Information Extraction
Constrained to predefined syntatic patterns  
Ambiguous extraction  
### Semantic role labeling
