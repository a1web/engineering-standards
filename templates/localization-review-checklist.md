# Localization review checklist

- [ ] User-visible text is in approved native localization resources.
- [ ] Accessibility-visible text is in approved native localization resources.
- [ ] Semantic identifiers are stable and meaningful.
- [ ] Translator context exists where English source text is ambiguous.
- [ ] Interpolation variables are named and documented.
- [ ] Plural and grammatical variants use native platform support.
- [ ] Dates, times, numbers, currencies, units, percentages, file sizes, relative times, and lists use locale-aware formatting.
- [ ] No localized sentence is assembled from concatenated fragments.
- [ ] Pseudolocalization or expanded-text smoke testing is covered.
- [ ] RTL layout behavior is covered where supported.
- [ ] Tests do not treat English wording as a shared behavior contract unless they explicitly test the English locale.
