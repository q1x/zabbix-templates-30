# zabbix-templates-30

## Introduction
This repository contains my efforts to create a repository based on the templates that were shipped with Zabbix 3.0.
The shipped templates are far from usable for a production deployment. They are also kept fairly basic as to not scare new users away from the product.
However, Zabbix has some very powerful features that, if used correctly, will bring your monitoring to a new level.

Thus, my goals with this repo are as follows:

- Create a logical and hierarchial structure in template linking to ease template maintenance
- Make templates as flexible as currently possible by utilizing linking, LLD, (context aware) user macros and calculated items where possible
- Set up a naming convention to ease working with templates from automated API scripts
- Utilize the description fields for templates, items and triggers to document behaviour inline as much as possible.
- Graph ALL the things, visualizing gathered data is key in your daily operations



