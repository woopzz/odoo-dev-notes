# <xpath ... position="move">

Sometimes you want to move an XML tag inside a view to another place.
You can use "move" as the position value for this purpose.
Unfortunately, it is poorly documented but you can find examples even in standard Odoo addons.
[Also there are several test cases.](https://github.com/odoo/odoo/blob/17.0/odoo/addons/base/tests/test_views.py#L553)

## An example of usage

```xml
<xpath expr="//group[@name='group1']" position="inside">
    <xpath expr="//field[@name='name1']" position="move"/>
</xpath>
```

The render engine finds a tag by an xpath condition `//field[@name='name1']` and moves it inside a tag that will be found by a condition `//group[@name='group1']`. So basically you put an xpath tag with position="move" to the place where you want to move your tag.
