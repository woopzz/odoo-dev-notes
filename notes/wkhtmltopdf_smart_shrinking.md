# Wkhtmltopdf smart shrinking

Sometimes you need to create a report with perfect-sized elements.
For example you have to print a grid of cards 3x4cm as many as A4 paper can fit.

What would you do?

First of all you create a new paperformat, select the A4 format, remove all margins (we require as much space as we can get),
set up DPI. Then according to the DPI you calculate the card width and height in pixels. Done.
You print a report and find out that cards have different size.

What's wrong?

The problem is a wkhtmltopdf feature called "smart shrinking". It changes the content size automatically.
It goes by default but there is an CLI option to disable it.

I would prefer the following solution...

```python
class IrActionsReport(models.Model):
    _inherit = 'ir.actions.report'

    @api.model
    def _build_wkhtmltopdf_args(self, paperformat_id, landscape,
                                specific_paperformat_args=None, set_viewport_size=False):
        res = super()._build_wkhtmltopdf_args(paperformat_id, landscape, specific_paperformat_args, set_viewport_size)

        if specific_paperformat_args.get('data-report-disable-smart-shrinking') == '1':
            res.append('--disable-smart-shrinking')

        return res
```

```xml
<template id="report_layout" inherit_id="web.report_layout">
    <xpath expr="//html" position="attributes">
        <!-- Make sure this option appear in "specific_paperformat_args" -->
        <attribute name="t-att-data-report-disable-smart-shrinking">data_report_disable_smart_shrinking</attribute>
    </xpath>
</template>

<template id="custom_report">
    <t t-call="web.basic_layout">
        <t t-set="data_report_disable_smart_shrinking" t-value="1" />
        <!-- ... -->
    </t>
</template>
```
