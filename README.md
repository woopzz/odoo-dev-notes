# Odoo Developer Notes

- [Naming](./notes/naming.md)
- [Default widget for optional pivot's measures](./notes/default_widget_for_optional_pivots_measures.md)
- [Time zone settings](./notes/time_zone_settings.md)
- [Wkhtmltopdf smart shrinking](./notes/wkhtmltopdf_smart_shrinking.md)
- [Set up fields.Selection:selection by method (supporting inheritance)](./notes/set_up_selection_by_method.md)
- [Advanced attribute modification](./notes/advanced_attribute_modification.md)
- [Hide menu items from from users of a particular group](./notes/hide_menu_items_from_group.md)
- [The attribute "force_save"](./notes/force_save_attr.md)
- [Yet another good idea to display a warning on a form view](./notes/yaga_to_display_a_warning_on_a_form_view.md)
- ["Clean action"](./notes/clean_action.md)
- ["Getting a record ID by its external identifier in qweb templates"](#getting-a-record-id-by-its-external-identifier-in-qweb-templates)

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
