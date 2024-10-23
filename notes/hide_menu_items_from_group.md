# Hide menu items from users of a particular group

```xml
<record id="ir_ui_menu_rule_sale_manager_cannot_read_configuration" model="ir.rule">
    <field name="name">Sales Manager cannot access "Configuration" tabs</field>
    <field name="model_id" ref="base.model_ir_ui_menu" />
    <field name="perm_read" eval="True" />
    <field name="domain_force">[
        '|',
        ('id', 'not in', [user.env.ref('sale.menu_sale_config').id, user.env.ref('crm.crm_menu_config').id]),
        (int(not user.has_group('sales_team.group_sale_manager')), '=', 1),
    ]</field>
</record>
```

So the condition fails if the curreny user participates in the sale manager group and he tries to access CRM / Configuration or Sale / Configuration.
And because this rule is global (no group was provided) if the condition fails, then the current user is not granted an access to these menu items.

There are many more cases where you can apply this trick, but remember that such rules are global and must be followed **always**.

## There is an important thing why this works

Odoo has a strict format for truthy and falsy leaves and you have to follow it.

```python
# odoo/osv/expression.py
TRUE_LEAF = (1, '=', 1)
FALSE_LEAF = (0, '=', 1)
```

As you can see there is difference in the first element. And fortunately Odoo calculates a domain you define here before matching it with this contstants.
