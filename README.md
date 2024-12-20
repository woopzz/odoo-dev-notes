# Odoo Developer Notes

- [CSS / SASS](./notes/stylesheets.md)
- [Naming](./notes/naming.md)
- [Default widget for optional pivot's measures](./notes/default_widget_for_optional_pivots_measures.md)
- [Time zone settings](./notes/time_zone_settings.md)
- [Wkhtmltopdf smart shrinking](./notes/wkhtmltopdf_smart_shrinking.md)
- [Set up fields.Selection:selection by method (supporting inheritance)](./notes/set_up_selection_by_method.md)
- [<xpath ... position="move">](./notes/position_move.md)
- [Advanced attribute modification](#advanced-attribute-modification)
- [Hide menu items from from users of a particular group](./notes/hide_menu_items_from_group.md)
- [The attribute "force_save"](#the-attribute-force_save)
- [A good way to display a warning on a form view](#a-good-way-to-display-a-warning-on-a-form-view)
- ["Clean action"](#clean-action)
- ["Getting a record ID by its external identifier in qweb templates"](#getting-a-record-id-by-its-external-identifier-in-qweb-templates)
- [How to update record values?](#how-to-update-record-values)

### Advanced attribute modification

```xml
<xpath expr="//div" position="attribute">
    <attribute name="class" add="my-fav-div" separator=" " />
</xpath>
```

> Use `remove` instead of `add` to cut out a substring.

### The attribute "force_save"

By default Odoo does not consider changes of readonly fields on views.
But you can force it to do that with the `force_save` attribute.

```xml
<field name="readonly_field>" force_save="1"/>
```

### A good way to display a warning on a form view

```xml
<!-- odoo/addons/base/views/res_partner_views.xml -->
<record id="view_partner_form" model="ir.ui.view">
    ...
    <field name="arch" type="xml">
        <form string="Partners">
            <div class="alert alert-warning oe_edit_only" role="alert" attrs="{'invisible': [('same_vat_partner_id', '=', False)]}">
                A partner with the same <span><span class="o_vat_label">Tax ID</span></span> already exists (<field name="same_vat_partner_id"/>), are you sure to create a new one?
            </div>
            ...
```

v15

### "Clean action"

You return an action out of a model method in order to run this action on the frontend.
Suppose, the action has `view_mode=tree,form`, so you expect to get a tree view.
But what you get is a form view for a new record.

In this situation you may want to "clean your action".
Try to wrap it up in [clean_action()](https://github.com/odoo/odoo/blob/15.0/addons/web/controllers/main.py#L232)
as [the endpoint for "/web/dataset/call_button"](https://github.com/odoo/odoo/blob/15.0/addons/web/controllers/main.py#L1329) does.

v15

### Getting a record ID by its external identifier in qweb templates

Use `%(external identifier)d`. It works in **attrs**, **domain**, **name**, probably somewhere else.

```xml
<!-- addons/base/views/ir_actions_views.xml -->
<button name="run" string="Run" type="object"
                                class="btn-primary"
                                attrs="{'invisible':['|', ('model_id', '!=', %(base.model_ir_actions_server)s), ('state', '!=', 'code')]}"
                                help="Run this action manually."/>

<!-- addons/base/views/res_lang_views.xml -->
<button string="Activate and Translate"
    name="%(base.action_view_base_language_install)d"
    type="action"
    class="oe_stat_button"
    icon="fa-refresh" />


<!-- addons/im_livechat/views/im_livechat_channel_views.xml -->
<field name="user_ids" nolabel="1" colspan="2" domain="[['groups_id', 'not in', %(base.group_portal)d]]">
```

v16

### How to update record values

I know three ways to do it:
1. The dot operator (or any other pythonic way to assign a value e.g. `setattr()`).
2. The method `update`.
3. The method `write`.

[update() goes through every field-value pair in values and changes every field one by one via square brackets.](https://github.com/odoo/odoo/blob/18.0/odoo/models.py#L6662)
Square brackets. Dot. Same things. Thus it's not really a choice, `update()` is just a shortcut.

The dot operator does some magic (idk, it looks like smth related to the cache and computed fields) and calls `write()` eventually.

As to the method `write` itself, it actually saves the changes.

Most of the time you want to use the dot operator and sometimes you need `write()`.

You might notice that `update()` applies changes field by field and it's not what you want when you have a constraint
that depends on a bunch of fields. In this cause you need to use `write()` directly to update all fields in one go.

Example:

```python
feature_x_enabled = fields.Boolean()
feature_x_required_field = fields.Char()

@api.constrains('feature_x_enabled', 'feature_x_required_field')
def _ensure_feature_x_fields_consistency(self):
    for record in self:
        if record.feature_x_enabled and not record.feature_x_required_field:
            raise ValidationError('Feature X has not been set up correctly.')
```

```python
# This doesn't work.
record.update({'feature_x_enabled': True, 'feature_x_required_field': 'smth'})

# This works!
record.write({'feature_x_enabled': True, 'feature_x_required_field': 'smth'})
```

v16
