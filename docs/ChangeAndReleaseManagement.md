Change and Release Management
=====

This document describes the Semantic Arts change and release management process for gist.

Review and Triage of Outstanding Issues
-----

- Semantic Arts ontologists meet regularly (twice monthly as of April 30, 2020) in order to review and triage GitHub issues. (Refer to [Contributing.md](Contributing.md) for guidelines on submitting an issue.)
- Issues are triaged in reverse chronological order.
- Issues are categorized in one of three ways:
  - **Will not implement**
    - Label as `closed:wontfix`, `closed:duplicate`, or `closed:can't reproduce`.
    - For duplicate issues, add the earlier issue number in a comment.
  - **Will implement**
    - Assign an implementer (could be external to SA under certain circumstances, such as when the issue submitted is external).
    - Assign or re-assign priority, impact level, and effort level. These may have been assigned by the submitter but may be revised as appropriate.
    - Tentatively assign to a release project. This may be reassigned to a later release project as needed.
    - Label as `status:triaged`.
    - Determine and document the resolution to be implemented. For small issues this can be done at the current review meeting; larger issues may require additional discussion, in which case the label `status:in review` should also be added. In such cases the assignee will prepare a summary of alternative implementations, pros and cons, rationales, etc. to be presented at the next meeting. This process will continue until reviewers agree on an implementation. Once an implementation is determined, apply the label `status:implementation specified`.
    - **Needs further review**
    - This case is similar to the will implement case, in that an assignee is designated to carry the discussion forward at a subsequent meeting; the difference is that here it is not yet agreed whether or how the issue will be addressed. Labels are applied as above.

Workflow: Implementation, Pull Requests, and Merges

- See [Contributing.md](Contributing.md).

Versions and Version Numbering
-----

Version numbers are of the form `X.x.x` (major.minor.patch). We follow [Semantic Versioning 2.0.0](https://semver.org/): as a guideline, but adjust as needed.

- **Major:** Non-backward-compatible (i.e., reasoning produces different results).
  - Examples: adding a restriction, domain, range.
  - Major changes should have a significant impact aside from technically modifying inferencing, if the latter is low-impact. For example, changing an equivalent class assertion pointing to a union class to a subclass axiom in order to allow new subclasses to be defined is not a major change.

- **Minor:** New, backward-compatible functionality. Includes _any_ addition to the ontology, even annotation properties. May constitute a large change to the ontology, such as addition of a new module.
  - Examples: adding a class or property; removing a restriction; adding annotation property `domainIncludes`; deprecation of a term (see following section Deprecation and Deletion Policy).

- **Patch:** No new functionality other than bug fixes.
  - Correction of an error, even if not backward-compatible, does not require a major release. The expectation is that users will not have implemented against an obvious error. This would be a patch.
  - Examples: Fixing a typo in an annotation; changing a property that was incorrectly defined as inverse functional rather than functional.
  
Deprecation and Deletion Policy
-----

- Terms to be removed, including changing a local name (which constitutes a removal plus an addition), enter a deprecation-deletion cycle.
- Deletion is a major change. If the next release is not a major release, terms must be deprecated rather than deleted.
- Deprecation is a minor change, since the deprecated term is still available for use. A patch release may not contain deprecations.
- Existing use of deprecated terms can be maintained, but users should not create new references to these terms. Rather, they should future-proof by using the new term, if there is one.
- Exception to deprecation: if the original term is referenced by other term definitions, as in a domain or range axiom, such that a change to that reference constitutes a major change, the term cannot be deprecated in a minor release. The change or removal must wait until the next major release, when the references to it can also be modified or removed.
- A deprecated term receives an `owl:deprecated` true assertion, and an `owl:equivalentClass` or `owl:equivalentProperty` assertion pointing to the new term.
- Deprecated terms are moved to the `gistDeprecated.ttl` file. To use a deprecated term, this file should be imported into the ontology. The deprecation file imports the rest of gist.
- A major release contains no deprecations. That is, all currently deprecated terms are deleted, and any other terms will be directly deleted rather than deprecated first.

Releases
-----

- Releases occur monthly, approximately on the last working day of the month, unless it is determined during a review meeting that there are not a significant number of substantive changes to warrant a release.
