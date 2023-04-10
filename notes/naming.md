# Naming

### ir.ui.view

```xml
    <!-- You want to define a form view for "my.model" model. -->
    <record id="my_model_view_form" model="ir.ui.view">
        <field name="name">my.model.view.form</field>
        <field name="model">my.model</field>
        <field name="arch" type="xml">
            <!-- ... -->
        </field>
    </record>

    <!-- You want to inherit "my_model_view_form" view of "module0" module. -->
    <record id="my_model_view_form_module0" model="ir.ui.view">
        <field name="name">my.model.view.form.module0</field>
        <field name="model">my.model</field>
        <field name="inherit_id" ref="module0.my_model_view_form" />
        <field name="arch" type="xml">
            <!-- ... -->
        </field>
    </record>
```
