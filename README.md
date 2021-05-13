# documentation

architecture decision records (ADRs), diagrams, explanation of processes, any other reference material and reports of past issues never solved

## table of contents

- [SSO users set up](./technologies/SSO.md)
- [architecture decision records](./architecture-decision-records/)
- [scenarios and threat models](./scenarios/)
- [learning and considerations on technologies](./technologies/)
- [scoutsuite reports](./scoutsuite/)
- [*deprecated bootstrap owners set up*](./technologies/bootstrap-owners.md)

## how to use this repo

few guidelines:

- **use markdown**: textual, simple and github renders it nicely for free
- **curate commit messages**: they're the tool to keep track of change so make them communicate
- **use consistent terminology**: allows simple search tools like "find in page" and `grep` to be highly effective
- **be concise**: nobody likes to read or write documentation when documents are as long as the lord of the rings
- **explain reasoning**: presprictive statements can become dogma, explaining reasonsing allows to challenge past decisions in a changing future
- **source for binaries**: if you have source files for binaries commit them as well to enable reproducibility (e.g. the `xml` export of a [draw.io](https://draw.io/) diagram as well as the `png`)
- **don't ask for permission, ask for reality check**: if you want to edit or add anything just go ahead and do it but make sure that what you're writing is understandable and not your unilateral opinion

