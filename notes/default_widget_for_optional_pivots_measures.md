# Default widget for optional pivot's measures

## TL;DR

Create a field tag without the "type" attribute.

```xml
    <pivot>
        <field name="optional_measure" widget="float_time" />
    </pivot>
```

## A long read

---

Assume you have the following model:

```python
class Example(models.Model):
    _name = 'example'

    user_id = fields.Many2one('res.users')
    date = fields.Date()

    most_important_hours = fields.Float()
    less_important_hours = fields.Float()
```

You get a task to create a pivot view for this model. It has to look like:

|        | 2022-05-25 | 2022-05-26 |
| ------ | ---------- | ---------- |
| User 1 |      00:00 |      02:00 |
| User 2 |      01:00 |      03:00 |

Every cell has to display the corresponding `most_important_hours`.

So you define the following pivot view:

```xml
<pivot>
    <field name="user_id" type="row" />
    <field name="date" type="col" />
    <field name="most_important_hours" type="measure" widget="float_time" />
</pivot>
```

It works fine.

Then you want to take a look at `less_important_hours` from time to time, but do not want to keep it displayed by default.

Fortunately, Odoo allows optional measures. You can find them in the "Measures" selector near the control panel.

But an optional measure comes up with a default widget based on the field type. You expect to see time here, but you get a float value. And it is quite inconvenient.

Looking at [Odoo v15 Pivot](https://www.odoo.com/documentation/15.0/developer/reference/backend/views.html#pivot)
we can see that the `type` attribute is optional but it is "row" by default. In practice a measure
without an explicit "type" still remains a measure. And now the pivot knows what
widget should be applied to this measure, but does not display it by default.

The final version:

```xml
<pivot>
    <field name="user_id" type="row" />
    <field name="date" type="col" />
    <field name="most_important_hours" type="measure" widget="float_time" />
    <field name="less_important_hours" widget="float_time" />
</pivot>
```
