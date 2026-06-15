# Branch Access Policy

main: Protected. Only the repo owner or release manager may merge into main via an approved Pull Request. No direct pushes permitted.

develop: Semi-protected. Developers may merge into develop via Pull Request after at least one peer review. This is the integration branch before release.

feature/*: Open to the assigned developer for that ticket only. All feature branches must be created from develop and merged back into develop via a Pull Request referencing the ticket ID.

Code flow: feature/* to develop to main. No code skips a stage without written justification and lead approval.
