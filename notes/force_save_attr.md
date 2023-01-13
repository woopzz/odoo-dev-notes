# The attribute "force_save"

By default Odoo does not consider changes of readonly fields on views.
But you can force it to do that with the `force_save` attribute.

```xml
<field name="readonly_field>" force_save="1"/>
```
