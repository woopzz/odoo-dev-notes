# Yet another good idea to display a warning on a form view

[Link v15](https://github.com/odoo/odoo/blob/15.0/odoo/addons/base/views/res_partner_views.xml#L156)

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
