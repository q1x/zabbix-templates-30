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
- When plausible, define host screens for easy access to relevant data.

## Template Hierarchy


### Modules


### Devices



## Naming conventions

### Templates

#### Visible name
Templates have 2 name fields in Zabbix, the 'Template name' and the 'Visible name'.
We use the 'Visible name' to represent the template in human readable form as it is the name a Zabbix operator will normally see when using the GUI.
The name always starts with the string 'Template' followed by a clear description of it's function.
The visible name can contains spaces to seperate words.

#### Template name
The more technical 'Template name' is the name that we will mostly use in scripts and within Zabbix configuration.
As such, we will define stricter rules in using this naming field.
Spaces will not be allowed, as this could break some poorly written scripts and makes parsing of the names more difficult.
An underscore '_' is used as a word seperator within the name. Template names will be written in lowercase.

Names consist of a number of fields and will always start with the first field containing the prefix 't_' to indicate the fact that we are dealing with a template. 
The second field contains a 3 character denominator for the template type (see [Hierarchy](#template-hierarchy)):

- Modules: mod
- Roles: rol
- Profiles: pro



### Host groups



### Usermacros
By Zabbix specifications, user macros are always written in upper case and cannot contain spaces (see [documentation](https://www.zabbix.com/documentation/3.0/manual/config/macros/usermacros)).
Therefor, we will use the underscore as a word seperator within the macro name.

The macro name consists of several fields. The first field specifies the functional field for the macro.

- I_:	Items (parameters)
- T_:	Triggers (function parameters and expressions)

