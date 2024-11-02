# CSS / SASS

### How to name classes

I like [the prefix that Odoo sugests for CSS classes](https://www.odoo.com/documentation/16.0/contributing/development/coding_guidelines.html#naming-conventions).
And it's the only guidance Odoo provides on selector naming. This leaves us to decide how to name selectors for pages, widgets, components and so on.

I suggest using [the BEM methodology](https://getbem.com).

---

Don't forget to add the prefix to class names!

```scss
.o-website-custom-list {
    ...
}
```

---

I use dashes to separate words for two reasons:
1. Bootstrap uses dashes as well. And since we use our classes alongside Bootstrap's, keeping the same separator helps maintain visual consistency.
I don't know why Odoo prefers snake case for classes.
2. BEM also uses dashes.

### How to name mixins

`mo_website_custom_name`

1. `mo` instead of `o` to make a distinction.
2. `website_custom` to avoid conflicts with mixins of other modules.
3. `name` as a mixin name.

Here we can use snake case. It doesn't really matter opposed to class names.

### Try to avoid nesting to "autocomplete" a selector name

Nesting is super useful in certain cases, like with pseudo-classes, and we should definitely use it there,
but excessive nesting makes the code harder to read and messier to work with.

I suggest keeping element and modifier classes separate and always using full names.

```scss
.o-website-custom-card {
    padding: 1rem;

    &:hover {
        .o-website-custom-card__link {
            color: green;
            text-decoration: underline;
        }

        .o-website-custom-card__button-next {
            display: block;
        }
    }
}

.o-website-custom-card__link {
    color: #000;
    text-decoration: none;
}

.o-website-custom-card__button-next {
    display: none;
}
```

### Use different classes for CSS and JS

If your JS logic doesn't affect styling, the classes you use should be separate as well.

I prefer adding the `js-` prefix to such classes, like `js-close`.
