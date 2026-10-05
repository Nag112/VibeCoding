---
description: "Plan an Azure DevOps work item into an OpenSpec change: snapshot via ado-agent, verify the digest against it, surface gaps for the user, then draft proposal/specs/design/tasks with acceptance criteria mapped to requirements, validate, and stop for review."
agent: "OpenSpec"
argument-hint: "<work item id> [project name]"
---

Plan the given Azure DevOps work item into an OpenSpec change. Do not implement anything.

**Input**: work item ID (required) and ADO project name (ask if missing and not inferable from context).

**Steps**

1. **Snapshot.** Delegate to `ado-agent` with `action: snapshot`, `id: <work item id>`, `project: <project>`. Wait for it to confirm the snapshot is saved and return the digest.

2. **Verify.** Once the snapshot file is saved and you've read it back, check the digest against the snapshot content. The snapshot is the source of truth if they disagree or the digest looks incomplete.

3. **Surface gaps.** List any ambiguities or missing acceptance criteria you find. Ask the user to resolve them before writing anything — do not proceed to step 4 until they respond.

4. **Create the change.** Name it `ado-<work item id>-<short-kebab-summary>` and write, in order:
   - **Proposal**: work item ID, URL, and the snapshot path and revision.
   - **Specs**: map each acceptance criterion to a requirement with scenarios, and cite which acceptance criterion it came from.
   - **Design**: only after the gaps from step 3 are resolved.
   - **Tasks**: derived from the design and requirements.

5. **Validate and stop.** Run `openspec validate <name> --json`, fix any reported issues, then stop for the user's review. Do not implement.
