# react-router-link-inactiveclassname
A fork of react router 4's Link component that supports inactiveClassName attribute

This relates to the NOFIX issue I raised on React Router's github: [TBD] strongly believe the NOFIX was an incorrect response for the following reasons:

When using a global/functional CSS strategy like Tachyons is it critical we do not rely on selector specificity and className order. At present, however, to style active and inactive `NavLinks` we do this:

```
<NavLink className="f5 black" activeClassName="red" />
```

Which renders as

```
<a class="f5 black" /> // <-- inactive
<a class="f5 black red" /> // <-- active
```

This happens to work because `.red` is after `.black` in the Tachyons style sheet. But if I'd chosen `.navy` instead of `.black` - which happens to be after `.red` - then the active style does nothing.

As you can see this is inherently brittle and furthermore browsers are [not required by the W3C spec](https://www.w3.org/TR/selectors/#specificity) to honour class order. You can get around this by creating custom css classes and invoking `!important` or increased specificity but this breaks a core principle of functional css.

Preferred would be:

```
<NavLink className="f5" activeClassName="red" inactiveClassName="black" />
```

Which would render as

```
<a class="f5 black" /> // <-- inactive
<a class="f5 red" /> // <-- active
```

giving you the results you'd expect.

Side note: from a general point of view, I've found that adding classes - as opposed to switching them - generally isn't scalable in a wide variety of cases not just Tachyons.
