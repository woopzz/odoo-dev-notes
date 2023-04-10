# "Clean action"

_v15_

You return an action out of a model method in order to run this action on the frontend.
Suppose, the action has `view_mode=tree,form`, so you expect to get a tree view.
But what you get is a form view for a new record.

In this situation you may want to "clean your action".
Try to wrap it up in [clean_action()](https://github.com/odoo/odoo/blob/15.0/addons/web/controllers/main.py#L232)
as [the endpoint for "/web/dataset/call_button"](https://github.com/odoo/odoo/blob/15.0/addons/web/controllers/main.py#L1329) does.
