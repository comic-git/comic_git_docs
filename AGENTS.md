# Documentation guidance

Write for people using the newest comic_git release. Clear, accurate instructions
matter more than preserving every older workflow or labeling every edit.

When a feature or behavior depends on a particular release, consider a small,
unobtrusive version note at the narrowest useful scope. Use wording that matches
the change, such as `> **Added in 1.1.1**` for a new capability or
`> **Changed in 1.1.0**` for different behavior. Confirm the version before
adding a note. Within a collapsible option description, put the note inside that
option's `<details>` block so its scope is clear. Avoid repeating the note in
subsections already covered by it.

Version notes are opportunistic reading aids, not a requirement for every docs
change. Do not add one for wording improvements, reorganized guidance, or an
existing feature that has merely gained a new explanation. Migration guides
already identify their version in the title and do not need a banner.

During later documentation updates, review nearby version notes and remove or
revise ones that are obsolete, misleading, or distracting. The docs may retire
outdated instructions; they do not need to preserve every historical version's
behavior indefinitely.
