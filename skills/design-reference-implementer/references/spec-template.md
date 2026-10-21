# Design spec template

Present this to the user before implementing. Keep it brief: tables and short lists, no essays. Write it in the user's language. Mark every value that was read from an image and not measured as **(est.)**.

```markdown
## Design spec: <target interface or component>

**Reference:** <what was provided: screenshot, prototype, logo...>
**Target:** <files or components that will change>
**Stack detected:** <framework + styling system + where tokens live>

### Refactor warning (only if at least one current element will be removed or change function)

This is a redesign of the interface. These current elements will not be present afterwards:

- <component or element>
- <component or element>

### Palette

| Role             | Value       | Maps to existing token |
| ---------------- | ----------- | ---------------------- |
| Background       | #... (est.) | `--bg` / new           |
| Surface          | #... (est.) | ...                    |
| Primary / accent | #... (est.) | ...                    |
| Text             | #... (est.) | ...                    |
| Muted text       | #... (est.) | ...                    |
| Border           | #... (est.) | ...                    |
| Error / success  | #... (est.) | ...                    |

Contrast notes: <pairs adjusted to reach WCAG AA, if any>

### Type scale

| Level           | Size       | Weight | Line height | Use |
| --------------- | ---------- | ------ | ----------- | --- |
| Display / H1    | ... (est.) | ...    | ...         | ... |
| H2              | ...        | ...    | ...         | ... |
| Body            | ...        | ...    | ...         | ... |
| Small / caption | ...        | ...    | ...         | ... |

Font: <family kept from the project, or the reference's feel if the project has none> (est.)

### Spacing and shape

- Base unit and scale: <e.g. 4px base, steps 4/8/12/16/24/32/48> (est.)
- Section padding, gaps between elements: ...
- Border radius: ...
- Elevation / shadows / borders: ...
- Layout: <grid or flex structure, max width, breakpoints used>

### Components

| Component    | Action            | Notes                  |
| ------------ | ----------------- | ---------------------- |
| <Button>     | Reuse and restyle | existing `Button`      |
| <Card>       | Reuse and restyle | ...                    |
| <Hero>       | New               | nothing similar exists |
| <Old widget> | Removed           | not in reference       |

### States to implement

- Interactive: hover, focus (visible ring), active, disabled
- Data: empty, loading, error
- Responsive: mobile, tablet, desktop behavior of the main layout

### Not changing

Content, data, copy, routes and behavior stay as they are.

### Estimates and open questions

- <anything that could not be measured or that you had to decide, such as mobile and tablet layouts when the reference shows only desktop>
```

End with a single clear question: do you approve this spec, or what would you like to change?
