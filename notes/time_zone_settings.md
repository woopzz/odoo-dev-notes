# Time zone settings

The Odoo database captures time in UTC.
Also Odoo supports different time zones and provides a setting in user profile.

Thus you can open your profile, select a time zone. Then Odoo puts it in your context (`env.context['tz']`).
And you will see time properly according to the time zone you set.

But apparently this setting works only on backend side.
On client side your browser preferences define your time zone.
[Odoo v15 proof.](https://github.com/odoo/odoo/blob/15.0/addons/web/static/src/legacy/js/core/session.js#L320)

As a result you send a time object (probably by setting up a datetime field) in one timezone from the frontend,
Odoo converts it in UTC and then displays it to you in another time zone (according to your profile's setting).
