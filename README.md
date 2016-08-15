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

The template hierarchy uses inherentence via template linking to model a monitoring profile which can be linked to a host.
The advantage of using template linking in multiple layers is that you can override usermacros on any of the levels to achieve a different behaviour for items or triggers that reside on a lower level in the chain.

### Task Templates

Task templates are the lowest layer of templates used in the template hierarchy. They contain items, triggers and graphs to only monitor a specific function of a host. For instance, the template `t_task_icmp_ping` only monitors host reachability via ICMP.


The usermacros used in the task templates provide sane defaults for item parameters and triggers to function. If needed, they can be overruled from higher level templates by overriding the [user macros](https://www.zabbix.com/documentation/3.0/manual/config/macros/usermacros).

It is allowed to create triggers on these templates, however, as they only give a very narrow insight into only a narrow part of the overal host state, it is recommended to only configure triggers with a severity level up to the Average severity level.

Graphs can be created that use the items from the template. Host screens are generally not useful on this level.

Preferably, [LLD](https://www.zabbix.com/documentation/3.0/manual/discovery/low_level_discovery) should be used to populate items, graphs and triggers.

### Duty Templates

Duty templates are used to combine data from lower task templates into a view of a particular service or duty of a host.
The duty level templates should not contain any items, except for [calculated items](https://www.zabbix.com/documentation/3.0/manual/config/items/itemtypes/calculated) that calculate a service state or item averages based on item values from task level items.

Triggers on this level in the template hierarchy will represent the state of a certain duty that the host has and therefor can be up to 'High' in severity level.

Graphs can combine items from multiple module level templates if so desired.
Also, it makes sense to create host screens on this level to give a broader overview of a hosts functioning although the Role templates might be better suited for this.

### Roles

Role templates merge the various services into a hosts role, for instance `LAMP server` or `Brand-X Access Switch`.

If needed, [calculated items](https://www.zabbix.com/documentation/3.0/manual/config/items/itemtypes/calculated) and triggers can be used to provide host level information. Triggers can go up to the disaster severity level.

The role level templates are perfect to configure a host level dashboard in the form of a host screen.

### Profiles

The sole purpose of the profile template is to add a layer in the hierarchy where we can override lower level usermacros to set default macros for a host role within a certain environment.
Profile templates *don't* contain any graphs, screens, items or triggers, all of those are inherited from lower level templates.

A profile template should be the only (exclusive) template that is linked to an actual host.

## Naming conventions

### Templates

#### Visible name
Templates have 2 name fields in Zabbix, the 'Template name' and the 'Visible name'.
We use the 'Visible name' to represent the template in human readable form as it is the name a Zabbix operator will normally see when using the GUI.
The name always starts with the string 'Template' followed by the template type (Task, Duty, Role or Profile) and a description of the template.
A double colon is uses as a field seperator. Use underscores instead of spaces.

e.g.: `Template::Task::ICMP_Ping`

#### Template name
The more technical 'Template name' is the name that we will mostly use in scripts and within Zabbix configuration.
As such, we will define stricter rules in using this naming field.
Spaces will not be allowed, as this could break some poorly written scripts and makes parsing of the names more difficult.
An underscore is used as a word seperator within the name. Template names will be written in lowercase.

Names consist of a number of fields and will always start with the first field containing the prefix `t_` to indicate the fact that we are dealing with a template. 
The second field contains a denominator for the template type (see [Hierarchy](#template-hierarchy)):

- Tasks: `task`
- Duties: `duty`
- Roles: `role`
- Profiles: `prof`


### Host groups
Host groups are used to group templates per template type (see [Hierarchy](#template-hierarchy)) and per category.

As host groups lack hierarchy (see [ZBXNEXT-1262](https://support.zabbix.com/browse/ZBXNEXT-1262)), we need to define another way to organize the templates.
To allow easier navigation of these groups, a double colon (::) is used to seperate the various fields within the group name.
This will also make it easier to parse the group names in API scripts until [ZBXNEXT-2934](https://support.zabbix.com/browse/ZBXNEXT-2934) is implemented.

e.g.: `Templates::Tasks::Generic`

### Usermacros
By Zabbix specifications, user macros are always written in upper case and cannot contain spaces (see [documentation](https://www.zabbix.com/documentation/3.0/manual/config/macros/usermacros)).
Therefor, we will use the underscore as a word seperator within the macro name.

The macro name consists of several fields. The first field specifies the primary functional level for the macro. This will allow us to quickly see where a macro is used.

- `I_`:	Items (parameters)
- `T_`:	Triggers (function parameters and expressions)

## Available Templates

Navigate the tree or follow these links to see the templates that are available within this repository.

- [Task templates](https://github.com/q1x/zabbix-templates-30/tree/master/tasks)
- [Duty templates](https://github.com/q1x/zabbix-templates-30/tree/master/duties)
- [Role templates](https://github.com/q1x/zabbix-templates-30/tree/master/roles)
- [Profile templates](https://github.com/q1x/zabbix-templates-30/tree/master/profiles)

