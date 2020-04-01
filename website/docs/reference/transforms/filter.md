---
last_modified_on: "2020-04-01"
component_title: "Filter"
description: "The Vector `filter` transform accepts and outputs `log` events allowing you to select events based on a set of logical conditions."
event_types: ["log"]
function_category: "filter"
issues_url: https://github.com/timberio/vector/issues?q=is%3Aopen+is%3Aissue+label%3A%22transform%3A+filter%22
sidebar_label: "filter|[\"log\"]"
source_url: https://github.com/timberio/vector/tree/master/src/transforms/filter.rs
status: "beta"
title: "Filter Transform"
---

import Fields from '@site/src/components/Fields';
import Field from '@site/src/components/Field';

The Vector `filter` transform
accepts and [outputs `log` events](#output) allowing you to select events based
on a set of logical conditions.

<!--
     THIS FILE IS AUTOGENERATED!

     To make changes please edit the template located at:

     website/docs/reference/transforms/filter.md.erb
-->

## Configuration

```toml title="vector.toml"
[transforms.my_transform_id]
  # General
  type = "filter" # required
  inputs = ["my-source-id"] # required

  # Condition
  condition.type = "check_fields" # optional, default
  condition."message.eq" = "this is the content to match against" # example
  condition."message.contains" = "foo" # example
  condition."environment.ends_with" = "-staging" # example
  condition."environment.starts_with" = "staging-" # example
```

## Options

<Fields filters={true}>
<Field
  common={true}
  defaultValue={null}
  enumValues={null}
  examples={[]}
  groups={[]}
  name={"condition"}
  path={null}
  relevantWhen={null}
  required={true}
  templateable={false}
  type={"table"}
  unit={null}
  >

### condition

The set of logical conditions to be matched against every input event. Only
messages that pass all conditions will be forwarded.



<Fields filters={false}>
<Field
  common={true}
  defaultValue={"check_fields"}
  enumValues={{"check_fields":"Allows you to check individual fields against a list of conditions.","is_log":"Returns true if the event is a log.","is_metric":"Returns true if the event is a metric."}}
  examples={["check_fields","is_log","is_metric"]}
  groups={[]}
  name={"type"}
  path={"condition"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  >

#### type

The type of the condition to execute.




</Field>
<Field
  common={true}
  defaultValue={null}
  enumValues={null}
  examples={[{"message.eq":"this is the content to match against"}]}
  groups={[]}
  name={"`[field-name]`.eq"}
  path={"condition"}
  relevantWhen={{"type":"check_fields"}}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  >

#### `[field-name]`.eq

Check whether a fields contents exactly matches the value specified.




</Field>
<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={[{"host.exists":true}]}
  groups={[]}
  name={"`[field-name]`.exists"}
  path={"condition"}
  relevantWhen={{"type":"check_fields"}}
  required={false}
  templateable={false}
  type={"bool"}
  unit={null}
  >

#### `[field-name]`.exists

Check whether a field exists or does not exist, depending on the provided value
being `true` or `false` respectively.




</Field>
<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={[{"method.neq":"POST"}]}
  groups={[]}
  name={"`[field-name]`.neq"}
  path={"condition"}
  relevantWhen={{"type":"check_fields"}}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  >

#### `[field-name]`.neq

Check whether a fields contents does not match the value specified.




</Field>
<Field
  common={true}
  defaultValue={null}
  enumValues={null}
  examples={[{"message.contains":"foo"}]}
  groups={[]}
  name={"`[field_name]`.contains"}
  path={"condition"}
  relevantWhen={{"type":"check_fields"}}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  >

#### `[field_name]`.contains

Checks whether a string field contains a string argument.




</Field>
<Field
  common={true}
  defaultValue={null}
  enumValues={null}
  examples={[{"environment.ends_with":"-staging"}]}
  groups={[]}
  name={"`[field_name]`.ends_with"}
  path={"condition"}
  relevantWhen={{"type":"check_fields"}}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  >

#### `[field_name]`.ends_with

Checks whether a string field ends with a string argument.




</Field>
<Field
  common={true}
  defaultValue={null}
  enumValues={null}
  examples={[{"environment.starts_with":"staging-"}]}
  groups={[]}
  name={"`[field_name]`.starts_with"}
  path={"condition"}
  relevantWhen={{"type":"check_fields"}}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  >

#### `[field_name]`.starts_with

Checks whether a string field starts with a string argument.




</Field>
</Fields>

</Field>
</Fields>

## Output

The `filter` transform accepts and [outputs `log` events](#output) allowing you to select events based on a set of logical conditions.
For example:


The `filter` transform is a simple conditional match, forwarding only those messages that pass all the conditions.
In this example, we drop all events that do not come from the host `gerry`:

```toml title="vector.toml"
[transforms.from_gerry]
  inputs = [ "somewhere" ]
  type = "filter"

  [transforms.from_gerry.condition]
    "host.eq" = "gerry"

[sinks.only_gerry]
  inputs = [ "from_gerry" ]
  type = "something"
```

Any event that does not match all of the conditions in the filter will be dropped by the transform.

## How It Works

### Complex Processing

If you encounter limitations with the `filter`
transform then we recommend using a [runtime transform][urls.vector_programmable_transforms].
These transforms are designed for complex processing and give you the power of
full programming runtime.

### Environment Variables

Environment variables are supported through all of Vector's configuration.
Simply add `${MY_ENV_VAR}` in your Vector configuration file and the variable
will be replaced before being evaluated.

You can learn more in the
[Environment Variables][docs.configuration#environment-variables] section.


[docs.configuration#environment-variables]: /docs/setup/configuration/#environment-variables
[urls.vector_programmable_transforms]: https://vector.dev/components?functions%5B%5D=program