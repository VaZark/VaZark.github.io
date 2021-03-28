---
title: Django Model Instance - Validation and Overriding
description: A quick look at how django validates models and how we can override it's default implementations
image: #
publishedAt: 2021-03-24
authors:
  - name: Vasanth M
    avatarUrl: /vaz_avatar.jpg
    link: https://twitter.com/vazarks
tags:
  - Django
  - Models
---

## Validation

- `Model.full_clean()`
  - `Model.clean_fields()`
  - `Model.clean()`
  - `Model.validate_unique()` 

**clean_fields()** validates individual fields and **validate_unique()** validates field level and Meta's *unique_together* uniqueness constraints. The **clean_fields()** and **validate_unique()** have a parameter *exclude* that allows us to skip validation for the mentioned fields.

Meanwhile, **clean()** is used to define custom model validation, and to modify attributes on your model if desired (eg. define values for certain fields)

**full_clean()** performs complete validation by invoking the above functions in the order mentioned.

`ValidationError` will be raised if any default validation fails.

## Overriding methods

### save

**save()** is used by both the create and update functionalities by the Model when updating the object. This can be used when custom validation or modification needs to be used. We can differentiate within *create* and *update* requests by checking if the model has a primary key. However, this can't leveraged when the primary key is user-defined and autogenerated by a ***UUIDField()***

When directly calling the **create** function from the model's Manager, the call bypasses the custom implementation. Make sure to call **save()** by creating the instance. An alternative can be to moved most of the logic into **clean()**

### Signals

Occasionally we need to perform some action that is tied to the changes in the DB but not necessarily associated with the DB/Model itself. In such cases, we can django's signal systems. The most common signals are,

- *pre-save*
- *post-save*
- *pre-delete*
- *post-delete*