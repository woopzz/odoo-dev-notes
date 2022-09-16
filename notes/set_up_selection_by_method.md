# Set up fields.Selection:selection by method
##### **supporting inheritance**

You need two methods:
- a method which returns selection pairs, you SHOULD override this one within other modules;
- a method which acts like a proxy and just calls the first method and returns its result, you SHOULD NOT override it.


```python
from odoo import fields, models


class MyModel(models.Model):
    _name = 'my.model'

    def _get_selection_pairs_colors(self):
        return [
            ('red', 'Red'),
        ]

    def get_selection_pairs_colors(self):
        return self._get_selection_pairs_colors()

    colors = fields.Selection(
        selection=get_selection_pairs_colors,
    )
```

```python
from odoo import fields, models


class MyModel(models.Model):
    _inherit = 'my.model'

    def _get_selection_pairs_colors(self):
        return super()._get_selection_pairs_colors() + [
            ('green', 'Green'),
        ]
```

An example from Odoo standard addons: [account.journal:bank_statements_source](https://github.com/odoo/odoo/blob/15.0/addons/account/models/account_journal.py#L161)
